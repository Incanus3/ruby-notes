Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-09-08T15:06:34+02:00

====== testing ======
Created Sunday 08 September 2013

http://guides.rubyonrails.org/testing.html

https://github.com/rspec/rspec-rails
https://github.com/thoughtbot/shoulda-matchers
https://github.com/jnicklas/capybara

https://github.com/fredwu/api_taster - visually testing Rails APIs

https://github.com/bmabey/database_cleaner - when testing js requests (e.g. by Capybara), you can't use transactional fixtures because the webserver runs in separate process that can't access the database when transaction is open in the test process, which leads to lock
http://fredwu.me/post/61571741083/protip-faster-ruby-tests-with-databasecleaner-and

http://stackoverflow.com/questions/8956816/comprehensive-guide-on-testing-rails-app
https://leanpub.com/everydayrailsrspec

http://gaslight.co/blog/6-ways-to-remove-pain-from-feature-testing-in-ruby-on-rails
http://re-factor.com/blog/2013/09/27/slow-tests-are-the-symptom-not-the-cause/

guard-livereload - reload browser when files change

===== factory_girl =====
* easily create factories for various types of objects (AR by default)
* FactoryGirl.attributes_for ignores associations, workaround helper:
'''
def build_attributes(*args)
  FactoryGirl.build(*args).attributes.delete_if do |k, v| 
    ["id", "created_at", "updated_at"].member?(k)
  end
end
'''
