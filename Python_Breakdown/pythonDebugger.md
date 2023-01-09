# Add breakpoint in script

- Python <3.7 -> import pdb; pdb.set_trace()
- Python 3.7+ -> breakpoint()

*Run the script normally eg python scriptname.py*

Once the breakpoint is reached there are a few options to interact with the code

- h: help
	+ Shows all functions and commands that are available

- w: where
	+ Shows where we're at in the script

- n: next
	+ executes next line

- s: step (steps into function)
	+ Same as next but goes into functions

- c: continue
	+ Continues executing until next breakpoint

- p: print
	+ p + variable name -> prints variable eg p a
	+ p + type(a) -> prints type of a eg p type(a)

- l: list
	+ Lists where we're in the code but more lines han w

- q: quit




# Notes ongoing
