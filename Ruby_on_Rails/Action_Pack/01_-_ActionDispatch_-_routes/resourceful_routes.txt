Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-09-08T01:47:07+02:00

====== resourceful routes ======
Created Sunday 08 September 2013

* resources directive takes name of the resource and options hash:
	* specify actions - options only, except
* it can also take a block to specify additional routes (member, collection), nested resources, etc.

* namespace :admin { resources :posts, ... }  - uzavre controllery do modulu, _path a _url budou mit prefix, path - /admin/posts
* scope '/admin' { resources :posts, ... } NEBO resources :posts, path: '/admin/posts' - controllery nebudou v modulu
* scope module: 'admin' { resources :posts, ... } NEBO resources :posts, module: 'admin' - uzavre controllery do modulu, ale path - /posts

===== nested resources =====
class Magazine < ActiveRecord::Base
  has_many :ads
end
 
class Ad < ActiveRecord::Base
  belongs_to :magazine
end

resources :magazines do
  resources :ads
end

cesty - /magazines/:magazine_id/ads/{,new,:id{,edit}}

===== shallow nested resources =====
* best practice - vyhnout se zanorovani cest, co to jde - ne /publishers/1/magazines/2/photos/3 (publisher_magazine_photo_url)
resources :posts do
  resources :comments, only: [:index, :new, :create]
end
resources :comments, only: [:show, :edit, :update, :destroy]
* THIS DOES THE SAME
resources :posts do
  resources :comments, shallow: true
end
-----
resources :posts, shallow: true do
  resources :comments
  resources :quotes
  resources :drafts
end
-----
shallow do
  resources :posts do
    resources :comments
    resources :quotes
    resources :drafts
  end
end

===== concerns =====
* predefined, reusable resources
concern :commentable do
  resources :comments
end
 
concern :image_attachable do
  resources :images, only: :index
end

resources :messages, concerns: :commentable
 
resources :posts, concerns: [:commentable, :image_attachable]

===== member routes =====
resources :photos do
  member do
    get 'preview'
  end
end

* OR
resources :photos do
  get 'preview', on: :member
end

* path - /photos/1/preview

===== collection routes =====

resources :photos do
  collection do
    get 'search'
  end
end
* OR
resources :photos do
  get 'search', on: :collection
end

* path - /photos/search
