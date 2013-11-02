Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-11-01T22:09:29+01:00

====== transactions ======
Created Friday 01 November 2013

* rolled back when exception occurs (e.g. insufficient balance on the withdrawn from account)
Account.transaction do
	account1.deposit(100)
	account2.withdraw(100)
end
* Active Record takes care of saving all the dependent child rows when you save a parent row
* Active Record is smart enough to wrap all the updates and inserts related to a particular save() (and also the deletes related to a destroy()) in a transaction
