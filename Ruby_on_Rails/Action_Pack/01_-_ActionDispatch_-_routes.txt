Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-09-08T01:46:23+02:00

====== 01 - ActionDispatch - routes ======
Created Sunday 08 September 2013

* rake routes

===== creating paths dynamically =====
http://api.rubyonrails.org/classes/ActionDispatch/Routing/UrlFor.html
http://api.rubyonrails.org/classes/ActionController/Redirecting.html - redirect_to uses url_for on its option hash (same as link_to and similar)

===== Creating Paths and URLs From Objects =====
resources :magazines do
  resources :ads
end

<%= link_to 'Ad details', magazine_ad_path(@magazine, @ad) %> # objects instead of ids

<%= link_to 'Ad details', url_for([@magazine, @ad]) %> # rails infers the right route from types

<%= link_to 'Ad details', [@magazine, @ad] %> # the same

<%= link_to 'Magazine details', @magazine %>

<%= link_to 'Edit Ad', [:edit, @magazine, @ad] %>

===== globbing =====
get 'photos/*other', to: 'photos#unknown' - sets params[:other] to all the rest

===== redirection =====
get '/stories', to: redirect('/posts')

get '/stories/:name', to: redirect('/posts/%{name}')

get '/stories/:name', to: redirect {|params, req| "/posts/#{params[:name].pluralize}" }
get '/stories', to: redirect {|p, req| "/posts/#{req.subdomain}" }

===== Routing to Rack Applications =====
Instead of a String like 'posts#index', which corresponds to the index action in the PostsController, you can specify any Rack application as the endpoint for a matcher:
match '/application.js', to: Sprockets, via: :all

As long as Sprockets responds to call and returns a [status, headers, body], the router won't know the difference between the Rack application and an action. This is an appropriate use of via: :all, as you will want to allow your Rack application to handle all verbs as it considers appropriate.

For the curious, 'posts#index' actually expands out to PostsController.action(:index), which returns a valid Rack application.

===== root =====
root 'pages#main'
-----
namespace :admin do
  root to: "admin#index"
end
 
root to: "home#index"
