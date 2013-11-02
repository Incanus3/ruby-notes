Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-09-08T14:43:28+02:00

====== EventMachine ======
Created Sunday 08 September 2013

http://timetobleed.com/eventmachine-scalable-non-blocking-io-in-ruby/ - perfektni prezentace EM od tvurce (a prispevatele do AMQP)

* EM.epoll - switch communication with scheduler from default select() to epoll() (linux only, use EM.kqueue on OSX)

* pokryva i spousteni reaktoru v novem vlakne - slide 52
'''
# start reactor in new thread
thread = Thread.current
Thread.new{
'''
	'''
	EM.run{ thread.wakeup }
	}
	# pause until reactor starts
	Thread.stop
	'''
* EM.next_tick - queue a proc to be executed on the next iteration (in reactor thread)
'''
n=0
do_work = proc{
'''
	'''
	if n < 1000
	'''
		'''
		do_something
		n += 1
		EM.next_tick(&do_work)
		'''
	'''
	end
	}
	EM.next_tick(&do_work)
	'''
* EM.tick_loop - wrapper around recursive calls to EM.next_tick

* EM.schedule
	* for thread-safety, you must use EM.schedule to call into the EM APIs from other threads

* obalit EM.stop do EM.next_tick - **resi problem s ukoncenim**
	* 1) zaruci, ze se EM.stop vola ve vlakne reaktoru
	* 2) resi problem s "optimalizaci" v event-loop (nize) - probudi reaktor
	* evented-spec pouziva take
* nektere examply obaluji i EM.add_(periodic_)timer do EM.schedule

 #eventmachine on irc.freenode.net

http://www.paperplanes.de/2011/4/25/eventmachine-how-does-it-work.html

https://github.com/mperham/evented - priklady vetsich kodu pouzivajici EM

===== EM and rails =====
http://stackoverflow.com/questions/3570392/how-do-eventmachine-rails-integrate
http://www.hiringthing.com/2011/11/04/eventmachine-with-rails.html#sthash.LhqThqJg.pIRdNCC3.dpbs

===== fibers =====

http://www.igvita.com/2009/05/13/fibers-cooperative-scheduling-in-ruby/

http://stackoverflow.com/questions/9052621/why-do-we-need-fibers - fibers, enumerators

http://www.igvita.com/2010/03/22/untangling-evented-code-with-ruby-fibers/

https://github.com/igrigorik/em-synchrony - synchronni komunikace s asynchronni knihovnou - vyuziva fibers a EM::Deferrable, callback (a errback) vola Fiber.resume, funkce na konci vola return Fiber.yield, takze blokuje (a prepne kontext), dokud deferrable nezmeni status (success, fail)
* obsahuje wrapper pro AMQP
https://github.com/igrigorik/em-http-request
https://github.com/igrigorik/em-synchrony/blob/master/spec/http_spec.rb - ukazuje synchronni praci s em-http-request (get, post, ...) a asynchronni, multithreaded praci s timtez pres multi rozhrani (aget, apost, ...)
https://github.com/igrigorik/em-synchrony/blob/master/lib/em-synchrony/amqp.rb - obsahuje fibre-aware wrapper k amqp - synchronizuje vetsinu asynchronnich amqp funkci
https://github.com/igrigorik/em-synchrony/blob/master/spec/amqp_spec.rb - spec - **nastudovat**

https://github.com/ruby-amqp/amqp/tree/master/spec/integration - specy amqp, spousta prikladu pouziti
* sdileni kanalu mezi vlakny, recovery pro ztrate spojeni, publisher confirms
* vsechny specy (vcetne integration) pouzivaji evented-spec, ktery se zda fungovat perfektne (ale nepousti event-loop v extra vlakne, coz u railsu potrebujeme, s tim se ostatne evidenetne prilis nepocita, neni to nikde poradne zdokumentovane)
