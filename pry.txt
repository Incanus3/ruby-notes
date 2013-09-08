Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-06-12T13:17:48+02:00

====== pry ======
Created Wednesday 12 June 2013

http://gaslight.co/blog/i-like-pry-but-dot-dot-dot

* all gems contained in pry-plus
* commands
	* ls - list current context - methods, variables, constants
		* ls -pM			list instance methods of Class including private
		* ls -m			list instance methods of object - object methods
		* ls -m			list class methods of class - object methods
		* ls -c			list constants
	* cd, cat [--ex], whereami
	* reset
	* show-doc (?)
	* show-source (show-method / $)
	* wtf? - show backtrace
	* play - The play command enables you to replay code from files and methods as if they were entered directly in the Pry REPL.
	* edit
		* edit [method]		edit method (defaults to first word of last input)
		* edit <file>[:<linenum>]
		* edit -c				edit current file
		* edit -n <file>		don't load file after edit (--no-reload)
		* edit --ex			edit where the last exception happened
		* -M - edit method defined on instances of object
		* -m - edit method defined on object itself
		* -s - edit super method (-ss - grandsuper)
* special variables
	* _ - last result
	* _file_ (or _dir_) - last file shown
	* _out_ - array of all return values of previous commands
	* _in_ - array of all subsequent inputs (strings)
* keyboard shortcuts
	* C-r - search in last commands
	* M-_ - copy last word from last input
	* C-d - exit or cd ..
* misc
	* add binding.pry to a point in code to start debugging there
	* command; - add semicolon to supress showing return value
	* .command - add dot to send command to shell

===== pry-rescue =====
* use rescue instead of ruby to jump to pry (like binding.pry) whenever unhandled exception is detected
* rescue rspec
* commands
	* cd-cause - jump to where the exception was raised (possibly call several times), use cd .. (ctrl+d) to go back
	* try-again - rerun code (rspec example, rails request) after edit
* in code
	* Pry.rescue do; test; end - control parts of code to rescue
	* Pry::rescued(e) - call in rescue block (on the exception)
* peeking
	* to interrupt and rescue a long-running example, run it by rescue and send it SIGQUIT (ctrl+\)
	* to rescue on other signal received by process, insert and run code by ruby command (**not rescue**)
ENV['PRY_PEEK'] = 'INT' # on SIGINT (ctrl+c)
require "pry-rescue"

===== pry-stack_explorer =====
* show-stack [-vHTc]
* up [number_of_frames | regex/string matching frame (fun name, etc.)]
* down [number|regex]
* frame [number as listed by show-stack|regex]

===== pry-debugger =====
* not thread safe
* step (step into call), next (step over call), finish (execute until current stack returns), continue (continue normaln execution)
* breakpoints:
'''
break SomeClass#run            Break at the start of `SomeClass#run`.
break Foo#bar if baz?          Break at `Foo#bar` only if `baz?`.
break app/models/user.rb:15    Break at line 15 in user.rb.
break 14                       Break at line 14 in the current file.

break --condition 4 x > 2      Change condition on breakpoint #4 to 'x > 2'.
break --condition 3            Remove the condition on breakpoint #3.

break --delete 5               Delete breakpoint #5.
break --disable-all            Disable all breakpoints.

break                          List all breakpoints. (Same as `breakpoints`)
break --show 2                 Show details about breakpoint #2.
break --help
breakpoints
'''

'''
Pry.commands.alias_command 'c', 'continue'
Pry.commands.alias_command 's', 'step'
Pry.commands.alias_command 'n', 'next'
Pry.commands.alias_command 'f', 'finish'
'''

