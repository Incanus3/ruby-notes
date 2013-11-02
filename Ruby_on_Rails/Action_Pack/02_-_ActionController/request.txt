Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-11-01T22:26:22+01:00

====== request ======
Created Friday 01 November 2013

* request (Rack::Request)
	* request_method - :delete, :get, :head, :post, :put
	* method - same as request_method, but returns :get instead of :head as these are functionally equivalent
	* get?, head?, post?, put?, delete?
	* xhr? (or xml_http_request?) - true if request issued by ajax helper (remote)
	* url
	* protocol, host, port, path, query_string - parts of the url
	* domain - last two components of host
	* host_with_port (separated by : )
	* port_string (e.g. 'http', 'https')
	* ssl?
	* remote_ip (as string, may have more than one when behind proxy)
	* env - You can use this to access values set by the browser, such as this: request.env['HTTP_ACCEPT_LANGUAGE']
	* accepts - array of mime::types - from the accept header
	* format - computed from accepts, falling back to html - used in respond_to { format <format> }
	* content_type - mime::type of the request
	* headers
	* body (io stream)
	* content_length (number of bytes in body)


* request.env['ENV_VAR']:
SERVER_NAME
PATH_INFO
REMOTE_HOST
HTTP_ACCEPT_ENCODING
HTTP_USER_AGENT
SERVER_PROTOCOL
HTTP_CACHE_CONTROL
HTTP_ACCEPT_LANGUAGE
HTTP_HOST
REMOTE_ADDR
SERVER_SOFTWARE
HTTP_KEEP_ALIVE
HTTP_REFERER
HTTP_COOKIE
HTTP_ACCEPT_CHARSET
REQUEST_URI
SERVER_PORT
GATEWAY_INTERFACE
QUERY_STRING
REMOTE_USER
HTTP_ACCEPT
REQUEST_METHOD
HTTP_CONNECTION
* could be used for terminal registration
