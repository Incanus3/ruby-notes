Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-06-27T19:25:09+02:00

====== zaseknuti reaktoru ======
Created Thursday 27 June 2013

https://groups.google.com/forum/#!topic/eventmachine/jSLOjpFdMfo
**Someone added an "optimization" to EM a while back to stop it waking up and calling select/epoll/kqueue every quantum. The idea behind the optimization is that it does no work if there's no work to do (i.e. sets the select/kqueue/epoll delay to the time at which the next event is expected). A consequence of this optimization is that events that EM is not aware of do not wake up the reactor. This includes partially closed connections and signals. **

**Next tick blocks are executed by the reactor by signaling the reactor on an internal socket, which is in the watched FD_SET, and as such, when you call next_tick it wakes up the sleeping reactor.**
