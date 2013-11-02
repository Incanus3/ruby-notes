Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-06-13T23:24:25+02:00

====== spolehlivost ======
Created Thursday 13 June 2013

===== Zajisteni spolehlivosti doruceni =====

==== na strane producera ====

=== publisher confirms ===
http://www.rabbitmq.com/confirms.html
Thirdly, if the connection between the publisher and broker drops with outstanding confirms, it does not necessarily mean that the messages were lost, so republishing may result in duplicate messages. Lastly, if something bad should happen inside the broker and cause it to lose messages, it will basic.nack those messages (hence, the handleNack() in ConfirmHandler).

podivat se na message TTL - v pripade neprijeti publisher confirm bude po nejake dobe treba poslat zpravu znovu, pokud ttl bude fungovat spolehlive, lze zarucit, ze se zprava nedoruci dvakrat

* vyhody - nezasekne producera, kdyz je spojeni pomale
* nevyhody - zprava mohla byt prijata brokerem, ale ten umrel drive, nez poslal confirm - zprava bude poslana producerem znovu -> duplikace;
	* slozitejsi implementace producera - musi predat metode send_message callback, ktery se zavola po potvrzeni prijeti zpravy brokerem
	* pri startu fs-managementu znovu odeslat zpravy ve stavu 'ceka na odeslani' - od kterych neprisel confirm

=== transakce ===
* umozni synchronni komunikaci s brokerem - channel.tx_commit blokuje vlakno, dokud nejsou vykonany vsechny akce (typicky publish) od deklarace channel.tx_select, nebo od posledniho commitu
* akce se zacnou provadet najednou teprve pri zavolani tx_commit
* vyhody - jednoduchost implementace producera - ve chvili, kdy send_message funkce vrati, je jistota, ze broker zpravu prijal (je ale potreba definovat error callback tak, aby vyhodil vyjimku,
	* jinak se pri preruseni spojeni neodeslou zpravy od posledniho commitu (volani tx_commit vrati take pri preruseni spojeni)
* nevyhody - blokuje vlakno, pokud je spojeni pomale, nebo docasne preruseno, zasekne producera

==== na strane consumera ====
* message acknowledgement:
	* bud potvrdit jen prijeti zpravy
	* nebo potvrdit uspesne zpracovani / nack pri neuspesnem

==== resource durability and message persistence ====
exchange = channel.direct("direct.exchange", :durable => true)
queue    = channel.queue("a.queue", :durable => true).bind(exchange)
exchange.publish('hello world', :routing_key => "events.hits.homepage", :persistent => true, :nowait => false) do
  puts "About to unsubscribe..."
  connection.close { EventMachine.stop }
end

==== message receipt acknowledgement ====
queue.subscribe(:ack => true) do |metadata, payload|
	channel1.acknowledge(metadata.delivery_tag, false) # pripadne metadata.ack
	# or
	channel.reject(metadata.delivery_tag)				# discard message, pripadne metadata.reject
	# or
	channel.reject(metadata.delivery_tag, true)			# requeue message, pripadne metadata.reject(:requeue => true)
end
