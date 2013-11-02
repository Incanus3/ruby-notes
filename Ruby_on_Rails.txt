Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-09-08T15:05:31+02:00

====== Ruby on Rails ======
Created Sunday 08 September 2013

===== lokalizace railsu =====
http://guides.rubyonrails.org/i18n.html
http://ruby-i18n.org/wiki
https://github.com/svenfuchs/rails-i18n



* storing money values
	* store as price in cents, to get price in dollars, do price_in_cents.to_d/100, to convert back, do price_in_dollars.to_d*100
* converting dates
	* to string - date.strftime("%Y-%m-%d %H:%M:%S")
		* if date can be nil, use date.try(:strftime, format)
	* from string - Time.zone.parse(string)
	* chronic gem - tons of time formats for parsing - relative values, etc.
