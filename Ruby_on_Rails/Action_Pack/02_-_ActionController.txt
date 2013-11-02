Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-09-18T13:10:41+02:00

====== 02 - ActionController ======
Created Wednesday 18 September 2013

* environment set by the controller (methods inherited from ActionController)
* action_name
* cookies
* headers (for response)
* params - can be indexed both by symbols and strings
* request
* response (normally managed by rails)
* session
* logger

* responding to the user (must be done exactly once - otherwise DoubleRenderError)
	* render a template (view) - default when no render, redirect, send_xxx, ... called
	* redirect
	* return a string directly
	* return 'nothing' (only headers, no body)
	* send data

* send_data(data, options)
	* options (suggestions to browser)
		* disposition - :inline/:attachment (default)
		* filename - used by browser when saving
		* status
		* type (default 'application/octet-stream')
		* url_based_filename boolean

* send_file(path, options)
	* options - buffer_size, disposition, filename, status, stream (boolean), type

* redirects
	* redirect_to(action: ..., options)
	* redirect_to(path)
	* redirect_to(:back) - redirects to HTTP_REFERER
	* use headers["Status"] = "301 Moved Permanently" for non-temporary redirect

===== Objects and Operations That Span Requests =====
* sessions - hash-like structure, stored as cookies by default (limited size, increases bandwidth, encrypted by default)
	* can hold any object, that can be marshalled (no functions, io objects, etc.)
	* can be stored on server (then only _session_id is stored in a cookie)
	* if you want to store model objects in cookies, you need to put model :name directive in the controller
	* session_store = :name
		* cookie_store (default), active_record_store, drb_store, mem_cache_store, memory_store, file_store

• cookies
	* cookies[:key] = value - creates temporary cookie
	* cookies.permanent[:key] = value
	* cookies.permanent.signed[:remember_me] = [current_user.id, current_user.salt]
		* uses ActionController::Base.cookie_verifier_secret
		* then use: user = User.authenticated_with_token(*cookies.signed[:remember_me])

* flash - hash-like structure, stored in session, available only in the next request
	* common to pass errors/notifications during redirect
	* flash.now - stores data in flash hash, but not in session - unavailable during subsequent requests
	* flash.keep - keep entries in flash for the next request (used when some intermediate request is issued)

* callbacks - called before/after/around actions - may provide authentication, logging, compression, ...
	* when beffore callback returns false, the callback chain (and the action itself) is broken
	* apend callback to chain
		before_action [name of method | object (filter method is invoked with controller as param)] [only: [actions] | except: [actions]] [block]
	* prepend callback to chain
		prepend_before_action
	* around callback - run in place of the action, if it calls yield, the action is called (or the next around callback)
	* when block given to callback definition, it recieves two params - controller object and proxy for the cation (call call() on it to run the original action)
	* when giving object to callback definition, it may be a class, then filter class method is invoked
	* callbacks are inherited from the controller - use skip_before_action / skip_after_action / skip_action to skip it


* ActionController::HideActions.hide_action(*args)  - mark methods as hidden actions - can't be called as actions (by ActionDispatch)
* AbstractController::Helpers.helper_method - define method as helper - accessible to views
					* .helper - takes (several) module(s) of helper methods (constants, symbols, strings), or (and) a block defining helper methods
	* in concerns define helper methods in the included block
* redirect_to @product = redirect_to product_path(@product)
	* [:edit, @product] = edit_product_path(@product)
	* [@category, @product], [@category, :edit, @product] - for nested resources
	* should work with link_to and similar too
* respond_with(object)
	* combined with class method respond_to :html,:xml, ...
	* GET - first, looks for a view - as render(object), if none is found, calls .to_<format> on object and renders the returned string
	* other methods - when there are errors on object, renders the correct action (new for create, edit for update)
		* when there are no errors, redirect_to object
		* with destroy, redirects to index
	* options
		* location - path to redirect when success
		* reponder - takes responder class - check out 
			* http://api.rubyonrails.org/classes/ActionController/MimeResponds.html
			* http://api.rubyonrails.org/classes/ActionController/Responder.html
			* http://weblog.rubyonrails.org/2009/8/31/three-reasons-love-responder/
			* https://github.com/plataformatec/responders
			* http://blog.plataformatec.com.br/tag/respond_with/
