Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-09-08T14:58:49+02:00

====== testing ======
Created Sunday 08 September 2013

http://martinfowler.com/articles/mocksArentStubs.html
* state/behavior verification
* classical/mockist testing
	* classical TDD - use real objects if possilbe
		* middle-out design
		* - fixture setup may be complex and/or expensive (but may be often reused)
		* - lots of fails when bug introduced on low-level/highly used object - may be hard to find the source
		* + integration tests (or unit tests using real objects as collaborators) may catch errors that waren't covered by unit tests
		* - state-based verification can lead to creating query methods only to support verification
	* mockist TDD - use mocks to test only the object under test
		* outside-in design
		* + easy fixture setup
		* + test isolation - easier to find the source of a failure
		* - coupling to implementation (also interferes with refactoring)
		* - may hide integration errors
		* - may delay the integration testing phase beyond safe point
		* + promotes 'Tell, don't ask' principle - encourages you to tell an object to do something rather than rip data out of an object to do it in client code
		* **+ favors role interfaces**
	* fine/coarse-grained tests - unit tests tend to bind to implementation, but integration tests tend to be too coarse
Avoiding method chains is also known as following the Law of Demeter

http://fredwu.me/post/59395419899/writing-sensible-tests-for-happiness

https://github.com/stympy/faker - generate random data - names, numbers, emails, streets, ...
http://robots.thoughtbot.com/post/23039827914/get-your-callbacks-on-with-factory-girl-3-3 - nice examples of more advanced factory_girl usage
