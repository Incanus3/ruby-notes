Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-09-08T15:50:29+02:00

====== digging in unfamiliar code ======
Created Sunday 08 September 2013

bundle open <gem> - open the gem in editor
Kernel.caller - returns calling  backtrace
object.method(:name).source_location

'''
self.class.ancestors.each do |klass|
  next unless klass.method_defined?(:method)
  puts klass.instance_method(:method).source_location
end
'''
