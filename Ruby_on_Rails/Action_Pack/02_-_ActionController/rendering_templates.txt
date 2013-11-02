Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-11-01T22:27:54+01:00

====== rendering templates ======
Created Friday 01 November 2013

* found in app/views/controller/action.type(.preprocessors) if not adjusted by ActionController.prepend_view_path(dir_path)
* render(hash)
	* () - renders default view - default when no render called or the action method is missing completely
	* (text: string)
	* (inline: string [, type: 'erb' | 'builder' | ...] [, locals: hash]
		class SomeController < ApplicationController
			if RAILS_ENV == "development"
				def method_missing(name, *args)
					render(inline: %{
						<h2>Unknown action: #{name}</h2>
						Here are the request parameters:<br/>
						<%= debug(params) %> })
				end
			end
		end
	* (action: action_name) - renders template for action **(but the action method isn't invoked - variables aren't set), don't confuse with redirect**
	* (template: name (e.g. 'blog/short_list'), [locals: hash])
	* (file: path (may be external), [layout: true (default false)])
	* (partial: name)
	* (nothing: true) - no body
	* (xml: stuff) - renders stuff as text, forcing the content type to be application/xml
	* (json: stuff, [callback: hash]) - renders stuff as JSON, forcing the content type to be application/json.
		Specifying :callback will cause the result to be wrapped in a call to the named callback function.
	* (:update) do |page| ... end - renders the block as an RJS template, passing in the page object
		render(:update) do |page|
			page[:cart].replace_html partial: 'cart', object: @cart
			page[:cart].visual_effect :blind_down if @cart.total_items == 1
		end
	* additional params - :status, :layout, :content_type
* render_as_string - same params, but returns the string - result isn't stored in the response (doesn't result in DoubleRender)
