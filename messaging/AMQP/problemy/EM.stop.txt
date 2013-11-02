Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-06-17T16:20:21+02:00

====== EM.stop ======
Created Monday 17 June 2013

I'm experiencing a pretty strange behavior here. This is my basic example code:

require 'amqp'

EM.run do
  session = AMQP.connect

  EM.add_timer(0.5) do
    puts "stopping"
    EM.stop
  end
end

puts "stopped"

after the timer shoots, the message "stopping" is printed, but the EM event loop never stops. It works perfectly if I comment the line calling AMQP.connect or when I add session.close to the timer block like this:

EM.run do
  session = AMQP.connect # do |session|
    # session.close
  # end

  EM.add_timer(0.5) do
    puts "stopping"
    session.close
    EM.stop
  end
end

It doesn't stop when I call session.close in the AMQP.connect block instead (as commented above) though. Moreover, when I uncomment the session.close call in the connect block, it doesn't stop even if I keep the session.close in the timer block, which seems pretty strange to me.


AMQP.stop musi dostat blok (i kdyby prazdny), jinak spadne (a jeste k tomu v jinem vlakne, takze vyjimka se ztrati, ale spadne reactor_thread)
