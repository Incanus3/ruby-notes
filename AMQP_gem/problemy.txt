Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-06-20T23:31:26+02:00

====== problemy ======
Created Thursday 20 June 2013

z evented-specu
* EventedSpec::SpecHelper::AMQPExample - spousti amqp session pred spustenim example, v done uzavira session, ukoncuje event-loop
	* pouziva delayed (ve kterem vola blok, ktery byl predan done) a next_tick, ve kterem ukoncuje AMQP a pak (v dalsim next_tick) i EM

evented-spec/evented_example/amqp_example.rb - AMQPExample < EMExample - run, done, finish_example
evented-spec-0.9.0/lib/evented-spec/evented_example/em_example.rb - EMExample - run_em_loop, finish_em_loop, timeout
evented-spec-0.9.0/lib/evented-spec/ext/amqp.rb - rozsireni AMQP modulu - start_connection  stop_connection


* pri volani AMQP.connect muze dojit k volani on_tcp_failure callbacku i proto, ze se reaktor ukoncil driv, nez prisla odpoved od brokeru
	* diky tomu muze napr. Consumer#subscribe_for_messages vyhodit ServerNotResponding
	* zvazit, zda to ma AMQPServer nejak resit, nebo je to rozumne chovani
