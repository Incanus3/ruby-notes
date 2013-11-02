Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-11-02T10:57:07+01:00

====== console ======
Created Saturday 02 November 2013

* rails c [environment]
	* --sandbox - wrap in sql transaction
* app object (
ActionDispatch::Integration::Session)
	* access to path and url helpers - e.g. app.products_path (ActionDispatch::Routing::RouteSet)
	* ActionDispatch::Integration::RequestHelpers
		* [**:get, :post, :patch, :put, :delete, :head**, :xml_http_request, :xhr, :follow_redirect!, :request_via_redirect, :get_via_redirect, :post_via_redirect,
		* :patch_via_redirect, :put_via_redirect, :delete_via_redirect]
	* ActionDispatch::Integration::Session#methods:
		* [:status, :status_message, :headers, :body, :redirect?, :path, :host, :host=, :remote_addr, :remote_addr=, :accept, :accept=,
		* :cookies, :controller, **:request, :response**, :request_count, :request_count=, :default_url_options, :default_url_options?, :default_url_options=,
		* :url_options, :reset!, :https!, :https?, :host!]
	* ActionDispatch::Integration::Runner:
		* [:app, :reset!, :get, :post, :patch, :put, :head, :delete, :cookies, **:assigns**, :xml_http_request, :xhr, :get_via_redirect, :post_via_redirect,
		* :open_session, :copy_session_variables!, :default_url_options, :default_url_options=, :respond_to?, :method_missing, :assert_tag,
		* :assert_no_tag, :find_tag, :find_all_tag, :html_document, :css_select, :assert_select, :count_description, :assert_select_encoded,
		* :assert_select_email, :response_from_page, :assert_recognizes, :assert_generates, :assert_routing, :with_routing, :assert_response,
		* :assert_redirected_to, :assert_dom_equal, :assert_dom_not_equal]
* helper object (ActionView::Base)
* controller object (ApplicationController)
* reload!
* disable printing of sql statements - ActiveRecord::Base.logger.level = 1 (anything > 0)
* _ - last result


* https://github.com/janlelis/irbtools
