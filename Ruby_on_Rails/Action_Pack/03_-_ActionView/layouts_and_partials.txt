Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-09-23T23:26:15+02:00

====== layouts and partials ======
Created Monday 23 September 2013

===== layouts =====
class StoreController < ApplicationController
	layout "standard", except: [ :rss, :atom ]
	# ...
end

class StoreController < ApplicationController
	layout :determine_layout
	
	# ...
	
	private
	
	def determine_layout
		if Store.is_closed?
			"store_down"
		else
			"standard"
		end
	end
end

def rss
	render(layout: false)
	# never use a layout
end
def checkout
	render(layout: "layouts/simple")
end

==== passing template-specific items to enclosing layout ====
* template (view)
<h1>Regular Template</h1>
<% content_for(:sidebar) do %>
	<ul>
		<li>this text will be rendered</li>
		<li>and saved for later</li>
		<li>it may contain <%= "dynamic" %> stuff</li>
	</ul>
<% end %>
<p>
Here's the regular stuff that will appear on
the page rendered by this template.
</p>

* layout
<!DOCTYPE .... >
<html>
	<body>
		<div class="sidebar">
			<p>
			Regular sidebar stuff
			</p>
			<div class="page-specific-sidebar">
➤					<%= yield :sidebar %>
			</div>
		</div>
	</body>
</html>

==== rendering partials ====
render(partial: 'article',
	    object: @an_article,
	    locals: { authorized_by: session[:user_name],
		          from_ip:
		          request.remote_ip })
* renders file _article.html.erb, @an_article is accessible via local variable article

<%= render(partial: "article", collection: @article_list) %>

<%= render(partial: "animal",
		    collection: %w{ ant bee cat dog elk },
		     spacer_template: "spacer") %>

<%= render("shared/header", locals: {title: @article.title}) %>
<%= render(partial: "shared/post", object: @article) %>

<%= render partial: "user", layout: "administrator" %>
<%= render layout: "administrator" do %>
	# ...
<% end %>

It isn’t just view templates that use partials. Controllers also get in on the
act. Partials give controllers the ability to generate fragments from a page
using the same partial template as the view. This is particularly important
when you are using Ajax support to update just part of a page from the
controller—use partials, and you know your formatting for the table row or
line item that you’re updating will be compatible with that used to generate
its brethren initially.

===== nested layouts =====
* app/views/layouts/application.html.erb:
'''
<html>
<head>
  <title><%= @page_title or "Page Title" %></title>
  <%= stylesheet_link_tag "layout" %>
  <style><%= yield :stylesheets %></style>
</head>
<body>
  <div id="top_menu">Top menu items here</div>
  <div id="menu">Menu items here</div>
  <div id="content"><%= content_for?(:content) ? yield(:content) : yield %></div>
</body>
</html>
'''


* app/views/layouts/news.html.erb:
''<% content_for :stylesheets do %>''
''  #top_menu {display: none}''
''  #right_menu {float: right; background-color: yellow; color: black}''
''<% end %>''
''<% content_for :content do %>''
''  <div id="right_menu">Right menu items here</div>''
''  <%= content_for?(:news_content) ? yield(:news_content) : yield %>''
''<% end %>''
''<%= render template: "layouts/application" %>''


