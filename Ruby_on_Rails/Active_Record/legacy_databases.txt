Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-09-11T17:22:30+02:00

====== legacy databases ======
Created Wednesday 11 September 2013

===== working with databases that don't follow AR naming guidelines =====
http://edgeguides.rubyonrails.org/active_record_basics.html#overriding-the-naming-conventions
http://jvans1.github.io/blog/2012/11/20/new-post/
http://www.slideshare.net/napcs/rails-and-legacy-databases-railsconf-2009
http://sl33p3r.free.fr/tutorials/rails/legacy/legacy_databases.html

* script/generate model tablename
* migrace - velikost typu - rails generate model user pseudo:string{30}, rails generate model product price:decimal{10,2}

create_table :jidelak do |t|
	t.string :s_o_v, :limit 1
end


class Client < ActiveRecord::Base
	set_table_name "client" OR self.table_name = "client"
	set_primary_key "client_id" OR self.primary_key = "client_id"
	
	has_one :firma, :class_name => "Firma", :foreign_key => "firma_key"
	alias_attribute 'name', 'person_name'
	
	pluralize_table_names = false # this shouldn't be needed
end

* self.primary_key=
* self.table_name=
Normally, Active Record takes care of creating new primary key values for
records that we create and add to the database—they’ll be ascending integers
(possibly with some gaps in the sequence). However, if we override the primary
key column’s name, we also take on the responsibility of setting the primary
key to a unique value before we save a new row. Perhaps surprisingly, we still
set an attribute called id to do this. As far as Active Record is concerned, the
primary key attribute is always set using an attribute called id . The primary_key=
declaration sets the name of the column to use in the table.

'''
book = LegacyBook.new
book.id = "0-12345-6789"
book.title = "My Great American Novel"
book.save
# ...
book = LegacyBook.find("0-12345-6789")
puts book.title # => "My Great American Novel"
p book.attributes #=> {"isbn" =>"0-12345-6789",
                  #    "title"=>"My Great American Novel"}
'''

Just to make life more confusing, the attributes of the model object have the
column names isbn and title — id doesn’t appear. When you need to set the pri-
mary key, use id . At all other times, use the actual column name.

Model objects also redefine the Ruby id() and hash() methods to reference the
model’s primary key. This means that model objects with valid IDs may be
used as hash keys. It also means that unsaved model objects cannot reliably
be used as hash keys (because they won’t yet have a valid ID).

Rails considers two model objects as equal (using == ) if they
are instances of the same class and have the same primary key. This means
that unsaved model objects may compare as equal even if they have different
attribute data. If you find yourself comparing unsaved model objects (which
is not a particularly frequent operation), you might need to override the ==
method.
