Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-09-08T15:25:02+02:00

====== debugging ======
Created Sunday 08 September 2013

http://www.ruby-doc.org/core-2.0/ObjectSpace.html
ObjectSpace module contains a number of routines that interact with the garbage collection facility and allow you to traverse all living objects with an iterator.
ObjectSpace also provides support for object finalizers, procs that will be called when a specific object is about to be destroyed by garbage collection.

http://www.scribd.com/doc/23548865/Debugging-Ruby - tracing
* Thread.abort_on_exception = true - **perfektni pro debug** - jinak muzou vyjimky casto uniknout - pokud jsou v jinem vlakne, nikdo se to nedozvi


https://github.com/deivid-rodriguez/byebug
https://github.com/deivid-rodriguez/pry-byebug - ruby-debugger -> pry-debugger doesn't work on ruby 2.0
