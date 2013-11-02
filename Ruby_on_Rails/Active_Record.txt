Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-09-17T00:08:57+02:00

====== Active Record ======
Created Tuesday 17 September 2013

* config/initializers/inflections.rb - configure translating singular forms to plural

===== columns =====
* Model.column_names
* Model.columns_hash
>> Order.columns_hash["pay_type"]
=> #<ActiveRecord::ConnectionAdapters::SQLite3Column:0x00000003618228
@name="pay_type", @sql_type="varchar(255)", @null=true, @limit=255,
@precision=nil, @scale=nil, @type=:string, @default=nil,
@primary=false, @coder=nil>
* special columns
	* created_at, created_on, updated_at, updated_on - automatically updated
	* id - primary key, assigned with save/create
	* xxx_id - foreign key
	* <table>_count - counter cache for table

===== associations =====
	* has_one, has_many, belongs_to (foreign key here)
	* has_and_belongs_to_many
* Rails implements many-to-many associations using an intermediate join table.
This contains foreign key pairs linking the two target tables. Active Record
assumes that this join table’s name is the concatenation of the two target
table names in alphabetical order
	* you need to create Model for jointable with 2 belongs_to relationships if you want to store additional information (e.g. amount of purchases)

===== constructors =====
	* new (create in-memory instance), create (persist immediately)
	* take hashes or array of hashes
	* no local variables
'''
Order.new do |o|
	o.name = "Dave Thomas"
	# . . .
	o.save
end
'''

'''
Order.new(
'''
	'''
	name: "Dave Thomas",
	email: "dave@example.com",
	address: "123 Main St",
	pay_type: "check"
	).save
	'''


===== reading data =====
* finders
	* find - takes (one or list of) primary keys, throws ActiveRecord::RecordNotFound if any of the keys isn't found
	* where
		* takes string with conditions, with possible wild cards, substituting subsequent arguments
			* ? - substitutes in the order they appear
			* symbols - substitutes from hash
		* or just a hash (keys must correspond to columns)
		* using like - User.where("name like ?", params[:name]+"%")
		* returns ActiveRecord::Relation
	* find_by_sql - takes string and possible wildcards, returns ActiveRecord::Relation
* Relation introspection - attributes() - hash, attribute_names() - list, attribute_present?() - boolean
* subsetting 
ActiveRecord::Relation:
	* first, last, all (to_a), implements each, map, ... by calling all() first
	* query doesn't get evaluated until one of these is used (can by further modified (scopes, etc.))
	* modifiers
		* order, limit, offset (used in combination to get chunks of the set)
		* select - only specific columns
		* joins - inserted into SQL immediately after the name of model's table and before any conditions
		* readonly - can't be stored back (automatically applied when joins or select is used)
		* group
		* lock - takes optional string (engine-specific), otherwise default exclusive lock is obtained
	* statistics
		* average, maximum, minimum, sum - take name of the column, may be combined with group (otherwise return single result)
		* count
* scopes
	* make method chains called on ActiveRecord::Relation reusable
	* take name and a function
* reloading data
	* when database is accessed by multiple processes/threads, in-memory instances may become stale
	* reload() method updates the attributes from database
	* or use a transaction

===== updating data =====
* save() - returns true/false
* update() - takes hash, saves immediately
* class methods
	* update(id, hash) or list of ids and hashes (then returns array)
	* update_all() on model or dataset

* save - return boolean, save! - raise exception
* create - always return AR instance (possibly invalid - non-persisted), create! - raise exception if invalid

===== deleting data =====
* class methods
	* delete(id / list of ids) - bypass validations and callbacks
	* destroy(id / list of ids)
	* delete_all(conditions) - bypass validations and callbacks
	* destroy_all(conditions)
* instance methods
	* destroy - deletes from database, freezes the object
