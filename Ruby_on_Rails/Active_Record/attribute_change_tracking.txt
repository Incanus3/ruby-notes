Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-11-01T22:08:48+01:00

====== attribute change tracking ======
Created Friday 01 November 2013

'''
object = Model.find_by_<attr> 'old_val'
object.<attr> = 'new_val'
object.changed?            #=> true
object.<attr>_changed?     #=> true
object.changed             #=> ['attr']
object.changed_attributes  #=> { 'attr' => 'old_val' }
'''

'''
object.changes             #=> { 'attr' => ['old_val','new_val'] }
object.<attr>_change       #=> ['old_val','new_val']
object.<attr>_was          #=> 'old_val'
object.<attr>_will_change  # sets dirty flag for <attr>
'''
