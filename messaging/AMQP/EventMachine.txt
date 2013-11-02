Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-06-08T17:05:51+02:00

====== EventMachine ======
Created Saturday 08 June 2013

* singleton methods
	* run, stop
	* add_[periodic_]timer
	* defer - run long-running work as bg job in thread pool
	* next_tick - run short-running work on next tick in reactor thread - usually recursively calls itself, can be used by bg thread to execute sth in reactor thread
* Deferrable - set success and failure callbacks, start the work, get the callback called with thre result when the worker signals success or failure
* Channel - publish/subscribe inter-thread communication
* Connection - handlers of IO/networking events subclass this
* Iterator - iterate over collection concurrently - proc for each worker (gets id) + proc called with results
* Queue - push processing onto reactor thread

https://github.com/eventmachine/eventmachine
https://github.com/eventmachine/eventmachine/wiki/General-Introduction
http://www.youtube.com/watch?v=mPDs-xQhPb0
http://www.igvita.com/2008/05/27/ruby-eventmachine-the-speed-demon/
http://dev.af83.com/2011/09/20/fighting-with-eventmachine.html
http://rubylearning.com/blog/2010/10/01/an-introduction-to-eventmachine-and-how-to-avoid-callback-spaghetti/ (+ linky dole)
http://www.slideshare.net/noverloop/defeating-blocking-io-with-eventmachine
http://pauldbergeron.com/ruby/eventmachine/event-machine-in-normal-ruby-or-rails-apps.html

* it's easy to unintentionally block the reactor by long running synchronous action (e.g. call to library fun)
* run IO in reactor thread, split work into chunks and run it using next_tick
* run long-running synchronous stuff in defer and set a completion callback

* co presne dela EM.run, stop, next_tick, defer, add_timer?
* EM.run(&block)
	* priserne napsane (dlouhe,necitelne)
	* begin
		* -> initialize_event_machine
		* -> add_timer(0,block)		# so the block can block the event-loop, but not the initialization
		* -> run_machine				# the thread is blocked in this call (this is where pry-rescue gets you, when you send SIGQUIT)
			* -> Reactor.instance.run
				* runs a loop { run_timers; crank_selectables; run_heartbeats } which breaks when @stop_scheduled
	* ensure
		* -> release_machine
* EM.stop
	* -> Reactor.instance.stop - set @stop_scheduled
* EM.next_tick
	* adds block to @next_tick_queue, calls signal_loopbreak if reactor_running?; signal_loopbreak sends data to UDPSocket - too low-level
* EM.defer(&block)
	* pushes block to @threadqueue
	* during first call, initializes threadpool - creates @threadpool_size threads, that run infinite loop popping procs from blocking queue, running them, pushing results on @resultqueue and signaling loopbreak
* EM.add_timer
	* -> add_oneshot_timer -> Reactor.install_oneshot_timer - @timers.add([Time.now + interval, uuid])
* EM.event_callback - called by run_timers (when timer fires)
	* spans two pages, too low-level

===== evented-spec =====
http://rubyamqp.info/articles/testing_with_evented_spec/
https://github.com/ruby-amqp/evented-spec
https://github.com/ruby-amqp/amqp/tree/master/spec/integration - amqp integration testy - evented-spec byl vyvinut kvuli nim, skvela dokumentace jak k evented-spec, tak k amqp
* EventedSpec::SpecHelper - em,amqp bloky
* EventedSpec::EMSpec, EventedSpec::AMQPSpec - obali kazdy example do em/amqp bloku
* default options, default_timeout
* hooks - em_before, em_after, amqp_before, amqp_after
* em block - done lze volat podminene, ve chvili, kdy je test splnen - zprava byla prijata, odeslani potvrzeno, kanal uzavren, ...
* done bere optional block, ktery se zavola po vyprseni (ma tedy cenu jen s casovym parametrem), muze obsahovat testy
* delayed(cas) blok (syntactic shugar pro EM.add_timer), muze obsahovat testy
* include EventedSpec::EMSpec/AMQPSpec muze byt uzavreny v dilcim describe/context bloku
