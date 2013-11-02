Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-11-01T22:11:39+01:00

====== callbacks ======
Created Friday 01 November 2013

* before_validation, (validation), after_validation, before_save, before_create/before_update, (insert/update), after_create/after_update, after_save
* before_destroy, (delete), after_destroy
* (before/after)_validation accept on: (:create / :update)
* after_find - called on ActiveModel instance after retrieving from db
* after_initialize - called on each new in-memory record - whether created by Model.new or Model.find(_by)
	* to get something like after_new, use after_initialize, :method, :if => :new_record?

* handlers - method name, block, callback objects (implementing methods with names corresponding to callback names)
	* when multiple handlers for the same event are specified, they will be invokend in the order specified
	* when handler returns false, subsequent handlers aren't called (and the action won't take place if it's a before hook)

* example of callback object
class Encrypter
	# We're passed a list of attributes that should
	# be stored encrypted in the database
	def initialize(attrs_to_manage)
		@attrs_to_manage = attrs_to_manage
	end
	# Before saving or updating, encrypt the fields using the NSA and
	# DHS approved Shift Cipher
	def before_save(model)
		@attrs_to_manage.each do |field|
			model[field].tr!("a-z", "b-za")
		end
	end
	# After saving, decrypt them back
	def after_save(model)
		@attrs_to_manage.each do |field|
			model[field].tr!("b-za", "a-z")
		end
	end
	# Do the same after finding an existing record
	alias_method :after_find, :after_save
end

class Order < ActiveRecord::Base
	encrypter = Encrypter.new([:name, :email])
	before_save encrypter
	after_save encrypter
	after_find encrypter
	
	protected
	
	def after_find
	end
end
* for performance reasons after_find and after_initialize are treated specially, Active Record won’t
	know to call an after_find handler unless it sees an actual after_find() method in the model class
