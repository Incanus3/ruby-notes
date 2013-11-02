Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-11-02T10:54:44+01:00

====== rack ======
Created Saturday 02 November 2013

http://railscasts.com/episodes/317-rack-app-from-scratch - build a rack app from the scratch

rails-footnotes - display various usefull info in page footer

===== rack middleware used in rails apps =====
* ActionDispatch::RemoteIp
	* env["action_dispatch.remote_ip"] = GetIp.new(env,self) - stores remote ip in request.env

* ActionDispatch::Callbacks
	* .before, .after(&block)

* ActiveRecord::ConnecitonAdapters::ConnectionManagement
	* calls ActiveRecord::Base.clear_active_connections! upon exception - could be used in apps using AR separately (message-monitor)

* controllers have their own middleware stack (empty by default)
	* you can use 'use <middleware>[, only: ..., except: ...] to use a middleware for some actions
	* because every controller action (returned by Controller.action(name)) is just a rack app on its own
	* => you can use some debugging/authentication, ... middleware just for certain controllers/actions
* you can use action_missing in controllers, similar to method_missing
