Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-11-02T11:49:50+01:00

====== Relation ======
Created Saturday 02 November 2013

http://blog.mitchcrowe.com/blog/2012/04/14/10-most-underused-activerecord-relation-methods/
* ActiveRecord::Relation
	* returned by .where, .group, .limit, ... methods
		* these only store the parameters for later
	* using Arel::Table inside Relation
		* arel - generates sql queries from dsl
	* to_a uses @klass.find_by_sql(arel.to_sql) to retrieve the data
	* arel is initialized in QueryMethods (included by Relation) #build_arel
		* builds up the query from values stored during the where, group, ... calls
* ActiveRecord scoping:
Model.where(...).scoping do
  Model.first ...      # all queries are scoped to the dataset given to scoping
  Model.where ...
end

* ActiveRecord::Calculations#pluck
	* while Relation.select(*columns) returns list of records with attributes restricted to columns
	* pluck immediately returns list of those values (simple list for single column, nested list for multiple columns)
	* Client.pluck(:id) = Client.select(:id).map { |c| c.id } = Client.select(:id).map(&:id) = Client.select(:id, :name).map { |c| [c.id, c.name] }

Book.where(:title => 'Tale of Two Cities').first_or_create do |book|
  book.author = 'Charles Dickens'
  book.published_year = 1859
end
