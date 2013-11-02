Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-09-23T23:25:53+02:00

====== form helpers ======
Created Monday 23 September 2013


http://api.rubyonrails.org/classes/ActionView/Helpers.html
* asset helpers, cache helper, form, form options, form tag, record tag, sanitize, translation
	* FormOptionsHelper - helpery pro vytvareni vyberu z kolekce - select, check boxy, radio buttony
collection_select(:post, :author_id, Author.all, :id, :name_with_initial, prompt: true) - call id on each item to get value, call name_with_initial to get text
* in the post sent by form, value will be accessible in params[:post][:author_id]
options_from_collection_for_select(@people, 'id', 'name')
* provides the list @people.map {|p| [p.name, p.id]} for select options - usable in form_for

http://juliankniephoff.wordpress.com/2011/03/23/using-form_for-in-helper-methods/
http://api.rubyonrails.org/classes/ActionDispatch/Routing/UrlFor.html

http://guides.rubyonrails.org/form_helpers.html
* form helpers
	* _tag
		* form_tag(path / hash spec (:controller, :action, ...), html options (:method, ...), &block)
			* form_tag('/search', method: 'get') { }
			* form_tag({controller: 'people', action: 'search'}, method: 'get', class: 'form') { }
		* label_tag(1 param name, text)
			* label_tag(:search_query, 'Search for:')
		* text_field_tag(1 param name)
			* text_field_tag(:search_query)
		* submit_tag(text)
			* submit_tag('Search')
		* <%= check_box_tag(:pet_dog) %>
		   <%= label_tag(:pet_dog, "I own a dog") %>
		* <%= radio_button_tag(:age, "child") %>                                       # these two are mutually exclusive (same name)
		   <%= label_tag(:age_child, "I am younger than 21") %>
		   <%= radio_button_tag(:age, "adult") %>                                      #
		   <%= label_tag(:age_adult, "I'm over 21") %>
	* no _tag
		* must be enclosed in form_tag (so they can be submitted)
		* text_field(2 param names - nested params) - presets value from object
			* text_field(:person, :name) - params[:person][:name], preset from @person.name
	* form_for(object (or its name), options, &block) - yields form builder object to the block, which implements methods, according to _tag-less helpers
		* but the first param is specified from object
		* options:
			* as - name of the first param key - form_for(@person, as: :client)
			* url - path or hash spec (controller, action)
			* method
			* namespace - prefix to make params unique
			* html - additional attributes (class, id)
	* fields_for - creates binding, similar to form_for, without generating the <form> tag

==== resource-oriented style ====
In the examples just shown, although not indicated explicitly, we still need to use the :url option in order to specify where the form is going to be sent. However, further simplification is possible **if the record passed to form_for is a resource**, i.e. it corresponds to a set of RESTful routes, e.g. defined using the resources method in config/routes.rb. In this case Rails will simply infer the appropriate URL from the record itself.
For example, for **existing record** @post:
<%= form_for @post do |f| %>
  ...
<% end %>

is equivalent to something like:
<%= form_for @post, as: :post, **url: post_path(@post), method: :patch**, html: { class: "edit_post", id: "edit_post_45" } do |f| %>
  ...
<% end %>

And for a new record
<%= form_for(Post.new) do |f| %>
  ...
<% end %>

is equivalent to something like:              (method: :post - default)
<%= form_for @post, as: :post, **url: posts_path**, html: { class: "new_post", id: "new_post" } do |f| %>
  ...
<% end %>

However you can still overwrite individual conventions, such as:
<%= form_for(@post, url: super_posts_path) do |f| %>
  ...
<% end %>

You can also set the answer format, like this:
<%= form_for(@post, format: :json) do |f| %>
  ...
<% end %>

For namespaced routes, like admin_post_url:
<%= form_for([:admin, @post]) do |f| %>
 ...
<% end %>

If your resource has associations defined, for example, you want to add comments to the document given that the routes are set correctly:
<%= form_for([@document, @comment]) do |f| %>
 ...
<% end %>

Where @document = Document.find(params[:id]) and @comment = Comment.new.

==== Removing hidden model id’s ====
The #form_for method automatically includes the model id as a hidden field in the form. This is used to maintain the correlation between the form data and its associated model. Some ORM systems do not use IDs on nested models so in this case you want to be able to disable the hidden id.

In the following example the Post model has many Comments stored within it in a NoSQL database, thus there is no primary key for comments.

Example:

<%= form_for(@post) do |f| %>
  <%= f.fields_for(:comments, include_id: false) do |cf| %>
    ...
  <% end %>
<% end %>

==== select boxes ====
* Select boxes in HTML require a significant amount of markup (one OPTION element for each option to choose from)
<%= select_tag(:city_id, '<option value="1">Lisbon</option>...') %>
* generates only the option tag
* use options_for_select to generate the option string
<%= select_tag(:city_id, options_for_select(...)) %>
* options_for_select(array (of objects or tuples [text,value]), default = nil
* **when **:include_blank** or **:prompt** are not present, **:include_blank** is forced true if the select attribute required is true, display size is one and multiple is not true**

* bound to object
# controller:
@person = Person.new(city_id: 2)
# view:
<%= select(:person, :city_id, [['Lisbon', 1], ['Madrid', 2], ...]) %>
* options are passed directly, not through string (generated by options_for_select)
* default is set automatically from person.city_id
* params[:person][:city_id]

# select on a form builder
<%= f.select(:city_id, ...) %>

=== Option Tags from a Collection of Arbitrary Objects ===
<% cities_array = City.all.map { |city| [city.name, city.id] } %>
<%= select_tag('person[city_id]',options_for_select(cities_array)) %>
# ==
<%= select_tag('person[city_id]',options_from_collection_for_select(City.all, :id, :name)) %>
# =~ (this also presets the default)
<%= collection_select(:person, :city_id, City.all, :id, :name) %>
* To recap, options_from_collection_for_select is to collection_select what options_for_select is to select.
* **Pairs passed to options_for_select should have the name first and the id second, however with options_from_collection_for_select the first argument is the value method and the second the text method.**

=== timezone and country select ===
* time_zone_select(:person,:time_zone)
* time_zone_options_for_select
* country_select extracted to country_select plugin

=== date and time selects ===
    Dates and times are not representable by a single input element. Instead you have several, one for each component (year, month, day etc.) and so there is no single value in your params hash with your date or time.
    Other helpers use the _tag suffix to indicate whether a helper is a barebones helper or one that operates on model objects. With dates and times, select_date, select_time and select_datetime are the barebones helpers, date_select, time_select and datetime_select are the equivalent model object helpers.
* barebone
<%= select_date Date.today, prefix: :start_date %>
# =>
<select id="start_date_year" name="start_date[year]"> ... </select>
<select id="start_date_month" name="start_date[month]"> ... </select>
<select id="start_date_day" name="start_date[day]"> ... </select>
# extraction
Date.civil(params[:start_date][:year].to_i, params[:start_date][:month].to_i, params[:start_date][:day].to_i)
* model
<%= date_select :person, :birth_date %>
# =>
<select id="person_birth_date_1i" name="person[birth_date(1i)]"> ... </select>
<select id="person_birth_date_2i" name="person[birth_date(2i)]"> ... </select>
<select id="person_birth_date_3i" name="person[birth_date(3i)]"> ... </select>
# params = {:person => {'birth_date(1i)' => '2008', 'birth_date(2i)' => '11', 'birth_date(3i)' => '22'}}
When this is passed to Person.new (or update), Active Record spots that these parameters should all be used to construct the birth_date attribute and uses the suffixed information to determine in which order it should pass these parameters to functions such as Date.civil.

* default - 5 years either side of the current year, use :start_year, :end_year options for select_date/date_select
* select_year, select_month, select_day, select_hour, select_minute, select_second
	* select_year(2009) #==
	* select_year(2009, field_name: 'year', prefix: 'date') #=> params[:date][:year]

===== advanced forms (multiple nested resources, etc) =====
http://guides.rubyonrails.org/form_helpers.html#using-form-helpers
http://guides.rubyonrails.org/form_helpers.html#building-complex-forms

http://thepugautomatic.com/2013/06/helpers/ - concat and capture

