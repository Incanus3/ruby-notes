Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-09-08T15:16:12+02:00

====== using ActiveRecord in non-Rails app ======
Created Sunday 08 September 2013

* ActiveRecord::Base.establish_connection uklada connection v hashi podle tridy, pri dotazu na ActiveRecord::Base.connection se prohledava hierarchie trid nahoru, dokud se nenajde v hashi spojeni, jinak skonci vyjimkou ActiveRecord::ConnectionNotEstablished
* ActiveRecord::Base.connected? vraci nil, pokud nebylo zavolano establish_connection, false mezi zavolanim establish_connection a prvnim dotazem na connection, true potom
* spojeni funguje i mezi vlakny (uklada se v singletonu)
	* opakovane volani establish_connection v dalsim vlakne muze vest k ConnectionNotEstablished v prvnim vlakne, ktere uz spojeni navazalo a pouziva ho, pravdepodobne proto, ze se sejde jeho pouziti (tudiz volani connection) s volanim establish_connection, behem nejz pravdepodobne chvili neni navazane zadne spojeni
