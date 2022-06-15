Setting up React
===============
There are two methods:
- [CRA](##-cra)
- [Non-CRA](##-non-cra)

## CRA
```
npm i -g create-react-app
create-react-app my-new-app

// or if you want to run it directly without installing the package globally

npx create-react-app my-new-app
```

Use the `create-react-app` command to automatically create a react app!

Gives you the broadest starting point for creating your react app. It's kept up to date, but will also include things you may not require, including logos, styling etc... it's a bit bloated!

### Ejecting
```
npm run eject
```
the eject script will stop hiding modules react has got installed under the hood and 'eject' those things into your project's package.json for everyone to see.

It also creates a config folder

## Non-CRA

### Setup Commands
```
npm i react-bootstrap bootstrap
npm i react-modal (a bootstrap alternative for modals presumably)
```

<p align="right">(<a href="#top">back to top</a>)</p>

- dependency array, saying I only want thing to be mounted once (???)

### public/index.html
- `<noscript>`This is a message that displays when JavaScript is disabled`</noscript>`

### src/index.js
This is your starting file, and it does the job of rendering your app.

It is not necessary to export this, or link it to the index.html as it will read the file and run it.


- `<React.StrictMode>` this will enable [Strict Mode](https://reactjs.org/docs/strict-mode.html), helpful for debugging. It will display more error messages, and prevent us from doing some things like using reserved words etc...

### src/App.js

<p align="right">(<a href="#top">back to top</a>)</p>
