# NODE & EXPRESS

## Setting up Node Rundown:
1. Inside your local repository set up your package manager:

    `npm init -y` or `npm init` to override defaults or add non-defaults
2. Create your entry point file:

    `index.js` unless otherwise specified
3. Write javascript file as normal, to run in terminal use:

    `node index.js`

## Importing and Exporting from modules

After creating your module file (example: `module.js`) to use functions in your entry point file you must export from the module and import into the entry point.

Using ES6:

1. Add `"type" : "module` to the package.json file
2. Export from module file:

    ```javascript
    //single
    export function functionName() { /*code*/ }

    //multiple functions
    export {
        //methods
    }

    //or 

    export default
    ```
3. Import into `index.js`:

    ```javascript
    import { /*methods*/ } from "./module.js";
    ```
4. Invoke functions as normal in `index.js`

Using CommonJS:

1. Export from `module.js`:

    ```javascript
    //single
    module.exports.functionName = () => //code   

    //multiple
    module.exports = {
        //methods
    }
    ```

2. Import into `index.js`:

    ```javascript
    const { /*methods*/ } = require("./module");
    ```

3. Invoke functions as normal in `index.js`

## Using Third Party Node Modules

Installing with npm:
1. In terminal `npm install <module>` 

    or `npm install -g <module>` if it is a package for global use

2. Double check module is installed with `npm ls` or the module version command

    example: `nodemon -v`
    ###
    If not installed globally, confirm the dependecies are in your package.json file

3. Consult module documentation for proper use

4. Create a `.gitignore` file to ensure you do not track and push your modules to GitHub.
    Make sure the file names included in .gitignore are spelled correctly for proper use

## Setting up Express:
1. Install express into your project:

    `npm i express`
3. Check for `express` listed as a dependency in `package.json` to confirm installation
2. Include Express using `const express = require('express')` in CommonJS

## Routing with Express
1. Set up as an express app with the following line:

    `const app = express()`
2. Set up your home route:

    ```javascript
    app.get('/', function(req, res) {
        res.sendFile(`${__dirname}/views/index.html`);
    });
    ```

    In this case, our index.html is nested in a directory named `views`, which will contain all page files.

    *Remember: `__dirname` allows you to access the complete file path no matter where it is currently stored.*

3. Set up other routes as required. For example:

    ``` javascript
    app.get('/about', (req, res) => {
        res.sendFile(`${__dirname}/views/about.html`);
    }); 
    ```
4. Set up the app to listen for a port in order to open your files.  Enter `localhost:<port>` in the browser url to view.  The page will only load after running Node.

    ```javascript
    app.listen(8000, () => console.log('starting server'));
    ```
    *Install Nodemon (`npm i -g nodemon`) to avoid restarting Node to update changes.  It is a good idea to log a starting statement in this case to confirm things are running.*

5. Set up your html files as usual, or...

## Using EJS View Templates

Instead of creating html files for your pages, you can use the templating package `EJS` to write in both html and javascript.

1. Install EJS

    `npm i ejs`

2. Instead of creating .html files, create .ejs files: 
    `index.ejs` vs. `index.html`

3. Include ejs in your app with the following:
    `app.set('view engine', 'ejs')`

4. Instead of using `.sendFile` employ the ejs method `.render`

    ```javascript
    app.get('/', function(req, res) {
        res.render('index');
    });
    ```
5. Set up the port in the same way and write your .ejs files as usual


### Accessing data using EJS

To send data from the request, you can `console.log(req)` to see how the information is stored.  Then you can access it the way you would an object or the response data in an api call.  

Example: `:date` at the end of the route says we'd like to store whatever is written there in the `req` data under the key 'date'.  This is put inside an object that has the key '`params`' inside the `req` object.  

```javascript
app.get('/blog/:date', (req, res) => {
    res.render('blog', { date: req.params.date });
});
```

By passing our own object with key '`date`', we can access the corresponding value in our `blog.ejs` file.  This can be done using `<%= date %>` inside the blog file. 
###
Any variable can be passed using the `.render` function, it need not be data found in from the request.

```javascript
app.get('/about', (req, res) => {
    res.render('about', { myVar: 'whatever value I want' });
});
```

### Using Javascript with EJS

To use javascript in the .ejs files, wrap every line with `<% %>`.  If you then wrap these lines in html tags, this text will display on the page.

```html
<h3>We're gonna count to ten:</h3>
<% for (let i =1; i<11; i++){ %>
    <p><%= i %></p>
<% } %>
```

The `<h3>` tag will add text as is usually expected.  The for loop logic will not be seen on the client side; however, the numbers 1-10 will be listed down the page as we have included the variable `i` and wrapped it in a `<p>` tag.

### Using partials in EJS

To streamline the creation of html elements, you can create and share `partials` between your various .ejs files.  Usually stored in a directory called partials, these files contain portions of the html than may want to appear on multiple pages on the same site.  Rather than edit in each file, you can maniupulate one copy and simply `include` it in location.

``` html
<%- include('../partials/nav.ejs') %>
```

Here, the nav bar, which should be shown on every page, exists as its own entity.  It is added to each page with the above line of code. 

### Using EJS Layouts

To further streamline the process in multipage webapps, you can install `express-ejs-layouts`.  This npm package will allow you to create an html layout that can be shared across multiple pages.  To use:

1. Install EJS Layouts:
    `npm i express-ejs-layouts`
2. Require layouts in your index.js file
    `const ejsLayouts = require('express-ejs-layouts')`
3. Include in your Express app
    `app.use(ejsLayouts)`
4. Create a `layout.ejs` file in the top level of your views folder
5. In this file, set up your html boiler plate and any features you want included in every page.  Unlike partials, You can set the same `<head>` information including stylesheet and title.

*To add style set up a `public` assest folder to contian your css and include in your index.js file*
*`app.use(express.static(__dirname + '/public'))`*


## Controllers

To avoid having all of your routes in your main `js` file. You can set up controllers to handle routes that branch off of similar url paths. For example, If you have a `/blog/create`, `/blog/view`, etc.

1. Create a `controllers` directory at the top level of your project
2. Create `<path>Controller.js` files for each of your paths with multiple subroutes.
3. In your main `js` require your routers to the proper path
    `app.use('/blog', require('./controllers/blogController'))`
4. In the controller file, set up and export your router
    ```javascript
        const blogRouter = require('express').Router();
        //routes
        module.exports = blogRouter;
    ```
5. Route the url path extension that branch off from the main path using router instead of app
    ```javascript
    blogRouter.get('/new', (req, res) => {
        res.render('blog/new');
    })
    ```
    This would go to the url path `/blog/new` and render your `new.ejs` file in the blog folder within the views folder.