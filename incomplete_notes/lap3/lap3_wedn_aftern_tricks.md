# Other

- ps $$ 
	+ Shows the command used (bash/zsh)

- see 20 most used commands
	+ `fc -l 1 | awk '{ CMD[$2]++; count++; } END { for (a in CMD) print CMD[a] " " CMD[a]*100/count "% " a }' | grep -v "./" | sort -nr | head -n 20 | column -c3 -s " " -t | nl`



*Order of loading*
.zprofile
.zshrc



# How to write an alias

`~/.zshrc`
_Shows zsh profile_

`~/.aliases` 
_Shows alias file_



### Write alias

`alias short="required command"`
*no space*
_example_
`alias vs="code ."`

- comments
	+ `#comment`


### Connect alias file to zsh file

`[[ -f "$HOME/.aliases" ]] && source "$HOME/.aliases"`
*Basically says if the file is there source it*

You can do the same with different files/filenames


# Snippets

Save snippets of code to be reusable in VSCode

{filename}.code-snippets
```
{
	"Print to console":{
		"scope": "javascript,typescript,javascriptreact",
		"prefix": "log",
		"body":[
			"console.log($0)$1;",
			"name = $2"
		],
		"description": "Log output to console"

	},

	"Other snippet"{...},
	"Add classname":{
		"scope": "javascript,typescript,javascriptreact",
		"prefix": "cn",
		"body":[
			"className=\"$1\"$0",
		],
		"description": "react class"
	},
	"Wrap in console log":{
		"scope": "javascript,typescript,javascriptreact",
		"prefix": "cw",
		"body":[
			"console.log($TM_SELECTED_TEXT)",
		],
		"description": "Wrap text in console.log"
	}


}
```
- Scope are languages that respond to this
- Prefix is what you type when coding
- Body is what comes up once you tab the snippet
	+ $n is where the cursor goes once the snippet is opened, goes from 0 to n


!configure snippets from settings





---





snippet-generator.app to develop snippet for VS/subl/atom

https://snippet-generator.app/
https://code.visualstudio.com/docs/editor/userdefinedsnippets





