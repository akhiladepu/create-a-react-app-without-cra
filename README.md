- This project was build from a [blog](https://blog.bitsrc.io/create-react-app-without-create-react-app-b0a5806a92)

## Step-1:
- Initialize npm for the packages required `npm init -y` and navigate to the folder.

## Step-2:
- Install webpack dependencies `npm i --save-dev webpack webpack-cli webpack-dev-server`

## Step-3:
- Install babel dependencies `npm i --save-dev babel-loader @babel/preset-env @babel/core @babel/plugin-transform-runtime @babel/preset-react babel-eslint @babel/runtime @babel/cli`

## Step-4:
- Install required Linters and path packages `npm i --save-dev eslint eslint-config-airbnb-base eslint-plugin-jest eslint-config-prettier path`

## Step-5:
- Install react and react-dom `npm i react react-dom`

## Step-6:
- Create folder called **public** in the root of the project. Inside that, create *index.html* ans add the following code to it.
- *index.html* is the final html file that renders on the browser.

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>React App without cra</title>
</head>

<body>
    <div id="root"></div>
    <script src="main.js"></script> <!-- The output file of webpack config-->
</body>

</html>
```

## Step-7:
- Create **src** folder and inside that create a file called *App.js*. Add the following code to it:
```
import React from "react";

const App = () =>{
    return (
        <h1>
            Welcome to React App that build without create-react-app.
        </h1>
    );
}

export default App;
```

## Step-8:
- Create an *index.js* file inside the **src** folder ( or at the root of project or anywhere you wish to have ). This will act as entry point for our webpack.
- If you are are using react version <=16, paste the following code.
```
import React from "react";
import reactDom from "react-dom";
import App from "./src/App"

reactDom.render(<App />, document.getElementById("root"));
```
- if you are using version 18, use the following code:
```
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
    <React.StrictMode>
        <App />
    </React.StrictMode>
);
```
- Since we are directly installing react, the current/latest version will be get installed.

## Step-9:
- Create *webpack.config.js*.
```
const path = require("path");

/*We are basically telling webpack to take index.js from entry. Then check for all file extensions in resolve. 
After that apply all the rules in module.rules and produce the output and place it in main.js in the public folder.*/

module.exports={
    /** "mode"
     * the environment - development, production, none. tells webpack 
     * to use its built-in optimizations accordingly. default is production 
     */
    mode: "development", 
    /** "entry"
     * the entry point 
     */
    entry: "./index.js", 
    output: {
        /** "path"
         * the folder path of the output file 
         */
        path: path.resolve(__dirname, "public"),
        /** "filename"
         * the name of the output file 
         */
        filename: "main.js"
    },
    /** "target"
     * setting "node" as target app (server side), and setting it as "web" is 
     * for browser (client side). Default is "web"
     */
    target: "web",
    devServer: {
        /** "port" 
         * port of dev server
        */
        port: "9500",
        /** "static" 
         * This property tells Webpack what static file it should serve
        */
        static: ["./public"],
        /** "open" 
         * opens the browser after server is successfully started
        */
        open: true,
        /** "hot"
         * enabling and disabling HMR. takes "true", "false" and "only". 
         * "only" is used if enable Hot Module Replacement without page 
         * refresh as a fallback in case of build failures
         */
        hot: true ,
        /** "liveReload"
         * disable live reload on the browser. "hot" must be set to false for this to work
        */
        liveReload: true
    },
    resolve: {
        /** "extensions" 
         * If multiple files share the same name but have different extensions, webpack will 
         * resolve the one with the extension listed first in the array and skip the rest. 
         * This is what enables users to leave off the extension when importing
         */
        extensions: ['.js','.jsx','.json'] 
    },
    module:{
        /** "rules"
         * This says - "Hey webpack compiler, when you come across a path that resolves to a '.js or .jsx' 
         * file inside of a require()/import statement, use the babel-loader to transform it before you 
         * add it to the bundle. And in this process, kindly make sure to exclude node_modules folder from 
         * being searched"
         */
        rules: [
            {
                test: /\.(js|jsx)$/,    //kind of file extension this rule should look for and apply in test
                exclude: /node_modules/, //folder to be excluded
                use:  'babel-loader' //loader which we are going to use
            }
        ]
    }
}
```

## Step-10:
- Create *.babelrc* file.
```
{
    /*
        a preset is a set of plugins used to support particular language features.
        The two presets Babel uses by default: es2015, react
    */
    "presets": [
        "@babel/preset-env", //compiling ES2015+ syntax
        "@babel/preset-react" //for react
    ],
    /*
        Babel's code transformations are enabled by applying plugins (or presets) to your configuration file.
    */
    "plugins": [
        "@babel/plugin-transform-runtime"
    ]
}
```

## Step-11:
- Update package.json file, Add the start and build scripts to it — line number 7 and 8.

```
"scripts": {
    "start": "webpack-dev-server ./src",
    "build": "webpack ./src",
}
```
## Final project structure 
![project structure](https://user-images.githubusercontent.com/81949743/178261248-20a29a1b-1aa3-43f9-8520-5a1e46f5cefd.png)

## Step-12:
- Building the code, run `npm run build`, to generate the final bundled code, which create the *main.js* file in the public folder.

## Step-13:
- Starting the development server `npm start`

## Changing build to production
- Change the line `mode: "production"` in *webpack.config.js* file.