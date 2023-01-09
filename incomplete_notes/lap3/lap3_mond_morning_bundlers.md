# Terminal tricks

- Ctrl A - E -> go to beginning - end of line
- Ctrl U -> remove everything
- Ctrl Y -> bring back everything
- Hold T (?) -> swap strings (?)
- Command , -> on mac see settings -> use option as meta key (?)
- Ctrl W -> remove word before cursor
- Alt D -> remove word after cursor
- Ctrl Shift - -> undo 


# WebPack and Bundlers configuration

WebPack -> Takes all modules and bundles them in static assets files

_Put all the .js .png etc in file src_

*Install*
`npm i -D webpack webpack-cli` 

- The cli is required to interact with webpack

*Run*
`webpack`

- Creates *dist* file

_OTHERWISE YOU NEED A CONFIGURATION FILE_
`webpack.config.js`

```js
const path = require('path')


module.exports = {
	entry: './src/index.js', //Entry point for bundling
	output: {
		filename: 'bundle.js' //Standard filename for bundle
		path: path.resolve(__dirname, './public') // standard configuration for react
	},
	mode: 'none'

}

```


*3 Modes*
- Production -> Things are minimised
- Development -> verbose for debug 
- None -> need to set up everything


_NOW YOU CAN RUN *WEBPACK* AGAIN_


---

#### Syntax for exporting with webpack

_Export_
```js

export default hello //Hello is function name in this case

````


_Import_
```js

import hello from './hello' //es6 syntax instead of require

```

require -> common js -> anything before es6
import -> modules import statement for es6+

---

### Types declaration
By default webpack only handles js and json files, you need to require a loader to import images, text, pdfs etc

`npm i -D file-loader`

*Modify webpack.config.js*
```js
const path = require('path')


module.exports = {
	entry: './src/index.js', //Entry point for bundling
	output: {
		filename: 'bundle.js' //Standard filename for bundle
		path: path.resolve(__dirname, './public') // standard configuration for react
	},
	mode: 'none',
	module: {
		rules: [
		{ test: /\.(png|jpg)$/, use: 'file-loader' } // if you find files with png/jpg extension (found with reg expression) use file-loader
		]
	}
}

```

*check regular expressions on RUBULAR*


---


## You can structure subfolders exclusively for certain components
Example:
You want to create a class which covers a certain function - you should add the css specific for those elements in the same folder, same for images and other dependencies


### Loader for CSS

*Install*
`npm i -D css-loader`
`npm i -D style-loader`

```js
const path = require('path')


module.exports = {
	entry: './src/index.js', //Entry point for bundling
	output: {
		filename: 'bundle.js' //Standard filename for bundle
		path: path.resolve(__dirname, './public') // standard configuration for react
	},
	mode: 'none',
	module: {
		rules: [
		{ test: /\.(png|jpg)$/, use: 'file-loader' }, // if you find files with png/jpg extension (found with reg expression) use file-loader
		{ test: /\.css$/, use: ['style-loader', 'css-loader'])}, // load CSS and style loader
		]
	}
}

// Css loader loads CSS, style loader loads CSS content in HTML head as style tag

```


### Add classes with THIS keyword
In order to do this, you need the ES6 syntax, _BABEL_ converts your ES6 to ES5- to be interpreted by the server/whatever framework


*Just like this*
`button.classList.add(this.buttonCssClass)`


*Need to install loader with babel*
`npm i @babel/core babel-loader @babel/preset-env @babel/plugin-proposal-class-properties --save-dev`

- Babel covers the newest syntax

```js
const path = require('path')


module.exports = {
	entry: './src/index.js', //Entry point for bundling
	output: {
		filename: 'bundle.js' //Standard filename for bundle
		path: path.resolve(__dirname, './public') // standard configuration for react
	},
	mode: 'none',
	module: {
		rules: [
		{ test: /\.(png|jpg)$/, use: 'file-loader' }, // if you find files with png/jpg extension (found with reg expression) use file-loader
		{ test: /\.css$/, use: ['style-loader', 'css-loader'])}, // load CSS and style loader
		
		//New bit
		{ test: /\.js$/, //for all js files
		  exclude: /node_modules/,
		  use: {
		    loader: 'babel-loader',
		    options: {
		      presets: ['@babel/preset-env'],
		      plugins: ['@babel/plugin-proposal-class-properties']
		    }
		  }
		}

		]
	}
}

```



## Different way of configuring (?)

Instead of adding the configuration option inside the config file you can do a separate one like this (not sure why you would tbh)

In `.babelrc` file 

```json
{
	"presets": ["@babel/preset-env"],

	"plugins": ["@babel/plugin-proposal-class-properties"]
}

```




## Add plugins

*Install first*
`npm install terser-webpack-plugin --save-dev`
`npm i -D mini-css-extract-plugin`

*Add plugin in appropriate section*
```js
const path = require('path')

//instantiate plugin declaration
const TerserPlugin = require('terser-webpack-plugin')
const MiniCssExtractPlugin = require('mini-css-extract-plugin');


module.exports = {
	entry: './src/index.js', //Entry point for bundling
	output: {
		filename: 'bundle.js' //Standard filename for bundle
		path: path.resolve(__dirname, './public') // standard configuration for react
	},
	mode: 'none',
	module: {
		rules: [
		{ test: /\.(png|jpg)$/, use: 'file-loader' }, 
		{ test: /\.css$/, use: [MiniCssExtractPlugin.loader, 'css-loader'])}, 
		{ test: /\.js$/, //for all js files
		  exclude: /node_modules/,
		  use: {
		    loader: 'babel-loader',
		    options: {
		      presets: ['@babel/preset-env'],
		      plugins: ['@babel/plugin-proposal-class-properties']
		    }
		  }
		}

		]
	},
	// Need to instantiate plugin and call it in plugin section
	plugins: [
	new TerserPlugin(),
	new MiniCssExtractPlugin({
		filename: 'auguste.css' // Brings all css to this filename
		// this clashes with style-loader so substitute it in css reg expression line
		// Given that style loader is no longer operative you need to declare CSS in your HTML
	}),

	//SAME AS BEFORE BUT TO AVOID CACHING!!!!
	new MiniCssExtractPlugin({
		filename: 'auguste.[contenthash].css' 

		// Creates a new CSS everytime you bring changes, to me not optimal at all as it stacks files up
	}) 
	// Next plugin automatically imports it


	]
}

```


*Automatic css import*

`npm i -D html-webpack-plugin`
This plugin basically structures the HTML based on all components declared in config file

```js
const HtmlWebpackPlugin = require('html-webpack-plugin');

//AFTER MiniCssExtractPlugin

new HtmlWebpackPlugin({
      title: 'Auguste', // Title of html file
      meta: {
        description: 'Webpack lecture' //Description of html file
      }
    })


// IF IN OUTPUT YOU ADD clean:true it will automatically remove all the older CSS

```




## Add favicon

Can add favicon.ico in public folder and add it to head

*OR*

In HtmlWebpackPlugin

```js

new HtmlWebpackPlugin({
      title: 'Auguste', // Title of html file
      meta: {
        description: 'Webpack lecture' //Description of html file
      },
      favicon: './src/fav.ico'
    })

```


# Change modes (this code is broken, double check each bit, probably has to do with the output path in the config file)

You can make multiple config files

Make one for each
`webpack.config.(dev|prod).js`


*Install*
`npm i -D webpack-dev-server` (?)


```js
const path = require('path')


module.exports = {
	entry: './src/index.js', //Entry point for bundling
	output: {
		filename: 'bundle.js' //Standard filename for bundle
		path: path.resolve(__dirname, './public') // standard configuration for react
	},
	mode: 'development',
	devServer:{
		static:{
			//double check this bit, should be ../dist
			directory: path.join(__dirname, './dist')
		},
		devMiddleware: {
			writeToDisk: true
		}, 
		compress:true,
		historyApiFallBack: true,
		open: true,
		hot: true,
		port: 9000
	}

}

```




## JSON SCRIPT
`{
"dev": "webpack-cli serve --config config/webpack.config.dev.js",
"build": "webpack --config config/webpack.config.dev.js"
}
`

