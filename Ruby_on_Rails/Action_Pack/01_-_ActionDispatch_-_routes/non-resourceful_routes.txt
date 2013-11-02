Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-09-08T02:32:24+02:00

====== non-resourceful routes ======
Created Sunday 08 September 2013

get ':controller(/:action(/:id))'
* map /photos/show/1 to PhotosController#show(1)
* map /photos to PhotosController#index (action is optional)

get ':controller/:action/:id/:user_id'
* You can't use :namespace or :module with a :controller path segment. If you need to do this then use a constraint on :controller that matches the namespace you require

get ':controller/:action/:id/with_user/:user_id'

get 'photos/:id', to: 'photos#show'

get 'photos/:id', to: 'photos#show', defaults: { format: 'jpg' }

get 'exit', to: 'sessions#destroy', as: :logout    - logout_path, logout_url
get ':username', to: 'users#show', as: :user

* namespace :admin { resources :posts, ... }  - uzavre controllery do modulu, _path a _url budou mit prefix, path - /admin/posts
* scope '/admin' { resources :posts, ... } NEBO resources :posts, path: '/admin/posts' - controllery nebudou v modulu
* scope module: 'admin' { resources :posts, ... } NEBO resources :posts, module: 'admin' - uzavre controllery do modulu, ale path - /posts

http://api.rubyonrails.org/classes/ActionDispatch/Routing.html - good examples of routes, e.g. 
'''
get '/articles/:year/:month/:day' => 'articles#find_by_id', constraints: {
  year:       /\d{4}/,
  month:      /\d{1,2}/,
  day:        /\d{1,2}/
}
'''
