Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-11-01T22:22:48+01:00

====== method lookup ======
Created Friday 01 November 2013

* ruby protected methods
	* can be called inside object (and descendants) with implicit recepient
	* can be called from methods of the same class with explicit recepient
	* can't be called from anywhere else
	* ruby 1.9 - respond_to?(:protected_method) #=> true (!)
	* ruby 2.0 - respond_to?(:protected_method) #=> false, cool, but!

	'''
	class A
	  def == a
	    puts a.respond_to? :zoom! #=> false despite the fact that we can actually call the method
	    puts a.zoom!
	  end
	
	  protected
	  def zoom!; :a; end
	end
	'''
