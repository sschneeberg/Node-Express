# Node-Express

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