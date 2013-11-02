Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-06-17T16:19:29+02:00

====== EM.run ======
Created Monday 17 June 2013

* testy se chovaji jinak nez produkcni beh co do spousteni EM event-loop
* vyzkouset nejdriv jednotlivou funkcionalitu s ruznym zpusobem spusteni event-loop, pak postupne kombinovat
* reaktor se blokuje, pokud se v EM.run vola AMQP.connect
	* pokud se v EM.run **po** AMQP.connect vola i add_timer, reaktor se rozbehne - to byl take pripad v testech, kdy byl v jednom EM.run volan subscribe a pote add_timer { send } - mozny workaround, ale tezko rict, jestli stabilni
	* pokud se AMQP.connect vola mimo EM.run, funguje v poradku - pravdepodobne nejjednodussi (a snad stabilni) reseni
[*] otocit pozice vlaken - pak bude mozne debugovat EM vlakno pres pry-rescue
	* spawnovat druhe vlakno, to ceka, nez se rozbehne EM, pak nastavi timery a joinuje
	* pak spustit EM.run v aktualnim vlakne a v nem AMQP.connect
* AMQP.connect -> AMQP::Client.connect -> AMQP.client.connect (AMQP.client => AMQP::Session) = AMQP::Session.connect << AMQ::Client::Async::EventMachineClient.connect (*)
	-> EventMachine.connect (as a handler suplies self) -> EventMachine.bind_connect, than it starts to be too low-level
		* (*) this is a line between two layers - EventMachine.connect handles the low-level connection details and calls 
				AMQ::Client::Async::EventMachineClient's callbacks when connection is received
			- AMQ::Client::Async::EventMachineClient implements the actual client-side AMQP protocol
* -> = calls
* => = returns
* << = inherited from
* since AMQP.connect just calls EventMachine.connect and handles the callbacks, it shouldn't block the reactor, unless Prace:friendly systems:server management:message-server:AMQP gem:EventMachine does
	* why does the call to add_timer or next_tick help?


