Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-09-17T17:51:26+02:00

====== Action Pack ======
Created Tuesday 17 September 2013

* ActionDispatch - routes requests to controller actions
* ActionController - converts requests into responses
* ActionView - formats these responses

* processing of requests
	* when request is routed to <controller>#<action> it looks for the following
		* public method called by the action
		* method_missing (called with the action name as first parameter)
		* if template named after <controller>/<action> exists, it's rendered directly
		* otherwise AbstractController::ActionNotFound is raised
