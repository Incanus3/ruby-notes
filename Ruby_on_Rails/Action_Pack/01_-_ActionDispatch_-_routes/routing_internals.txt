Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-10-20T14:51:51+02:00

====== routing internals ======
Created Sunday 20 October 2013

* routes.rb
Objednavky::Application.routes.draw do # Objednavky::Application defined in config/application, inherits Rails::Application
* Rails::Application is defined in railties/lib/rails/application, inherits Rails::Engine
	* .routes creates (and memoizes) ActionDispatch::Routing::RouteSet.new
* RouteSet
	* .draw(&block)
		* creates Mapper.new(self)
		* passing block with a parameter is deprecated
		* depending on default_scope, evaluates the block in context of mapper (instance_exec) either directly or in scope block
* Mapper
	* .new(route_set) - @set = route_set
	* match -> decomposed_match -> add_route
		* to: 'controller#action'
		* **or to: any Rack application (which is a callable, co can just be a proc)**
		* Controller.action('index') - returns rack app (callable), that can be called with some env
		* so 'products#index' is expanded to ProductsController.action('index') and called during dispatch
		* **or to: redirect(path)**
	* add_route
		* creates new Mapping object - takes set, scope, path, options, normalizes them and computes requirements, conditions and defaults
		* calls to_route on it - returns app, conditions, requirements, defaults, options[:as], options[:anchor]
		* passes these @set.add_route
	* root is just calling match '/', as: :root
	* submodules
		* Base - root, **match**, mount
		* HttpHelpers - get, post, patch, put, delete - delegate to match
		* Scoping - **scope** -> controller, namespace, constraints, defaults
		* Resources - resource(s), collection, member, new, nested, namespace, shallow(?), match, root
		* Concerns - concern(s)
