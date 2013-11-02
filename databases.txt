Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-11-02T10:20:26+01:00

====== databases ======
Created Saturday 02 November 2013

https://github.com/ianwhite/orm_adapter
* Provides a single point of entry for popular ruby ORMs. Its target audience is gem authors who want to support more than one ORM.
* used by devise to be orm-independent

''User # is it an ActiveRecord, DM Resource, MongoMapper or MongoId Document?''

''user_model = User.to_adapter''
''user_model.get!(1)                      # find a record by id''
''user_model.find_first(:name => 'fred')  # find first fred''
''user_model.find_first(:level => 'awesome', :id => 23)''
''                                        # find user 23, only if it's level is awesome''
''user_model.find_all                     # find all users''
''user_model.find_all(:name => 'fred')    # find all freds''
''user_model.find_all(:order => :name)    # find all freds, ordered by name''
''user_model.create!(:name => 'fred')     # create a fred ''
''user_model.destroy(object)              # destroy the user object''

* use !! to return boolean according to some non-boolean value, but hide the actual value
'''
def valid_params_request?
  !!env["devise.allow_params_authentication"]
end
'''
