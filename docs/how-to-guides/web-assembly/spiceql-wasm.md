# Using SpiceQL WebAssembly Bindings

SpiceQL is an inteface for querying data from NAIF SPICE Kernels.  It has WebAssembly (Wasm) Bindings so it can be used in Web, JavaScript, and Node.js environments.


With a kernel loaded in to the virtual filesystem, SpiceQL can be used to let a web read data from it.  

## Included Files

SpiceQL Wasm bindings are distributed via [NPM](https://www.npmjs.com/package/@usgs-astrogeology/spiceql).  The distribution includes [these files](https://www.npmjs.com/package/@usgs-astrogeology/spiceql?activeTab=code):

```sh
spiceql/
├── dist/
│   ├── naifspice.js        # raw CSPICE marshaller
│   ├── spiceql.js          # Wrapper (Import this one!!!)
│   ├── spiceql_wasm.data   # Config DB, Leap-second Kernel, naifspice signatures
│   ├── spiceql_wasm.js     # Emscripten Loader (ES Module)
│   └── spiceql_wasm.wasm   # WebAssembly Binary
├── LICENSE.md
├── README.md
└── package.json
```

## Installation

Create a project with [NPM](https://docs.npmjs.com/about-npm), or use [jsDelivr](https://www.jsdelivr.com) for a quick start.

=== "NPM"

    [NPM](https://docs.npmjs.com/about-npm) is a package manager, ideal for setting up a project with multiple dependencies.  Use [Vite](https://vite.dev/guide/) and install USGSCSM to setup a quick project:

    1.  **Get a Module Bundler** (See [webreference](https://webreference.com/javascript/advanced/module-bundlers/), [wikipedia](https://en.wikipedia.org/wiki/Module_bundler))

        Needed to resolve import path.  If starting from scratch, setup Vite:  

        ```sh
        # In your terminal in the project folder:
        npm create vite@latest
        ```

    2.  **Install SpiceQL**  

        ```sh
        # In your terminal in the project folder:
        npm install @usgs-astrogeology/spiceql
        ```

    3.  **Import SpiceQL** in your javascript

        ```js title="yourscript.js"
        import spiceql from "@usgs-astrogeology/spiceql/"
        ```

    4.  **Write Code with SpiceQL**

        See [Javascript - Quick Example](#examples) below.

    5.  **Run A Server**
    
        If you're using Vite, by default, you should be able to run a local server with:  
        ```sh
        # In your terminal in the project folder:
        npm run dev
        ```

=== "jsDelivr (CDN)"

    [jsDelivr](https://www.jsdelivr.com) is a Content Delivery Network. It fetches SpiceQL from NPM (or GitHub) for a quick and easy import with no setup.  It may add a little bit of loading time though.

    ```js title="yourscript.js"
    import { loadSpiceQL } from 'https://cdn.jsdelivr.net/npm/@usgs-astrogeology/spiceql/dist/spiceql.js'
    ```

=== "Manual Download"

    Download the above files from the 
    [release page](https://github.com/DOI-USGS/spiceql/releases), 
    and place the files in your project directory.  (For this example, in a `scripts` folder.)

    ```js title="yourscript.js"
    import { loadSpiceQL } from "./scripts/spiceql.js"
    ```

??? warning "Wasm requires running a server and using `<script type="module" ...>`."

    #### Server

    Running a server is required to use web assembly modules. 
    Accessing a page with USGSCSM via `file://` won't work. 
    [Use NPM + Vite to setup a local server](#__tabbed_1_1).
    Or, if you have python, you can run a simple local server:

    ```sh
    # In your terminal in the project folder:
    python3 -m http.server 8080
    ```


    #### Module

    Use `type="module"` in the script tag in your HTML:

    === "HTML with separate .js file"

        ```html title="index.html"
        <script type="module" src="/src/yourscript.js"></script>
        ```

        ```js title="yourscript.js"
        // In your javascript:
        import { loadSpiceQL } from 'https://cdn.jsdelivr.net/npm/@usgs-astrogeology/spiceql/dist/spiceql.js';
        const spiceql = await loadSpiceQL();
          // ...
        ```

    === "HTML with embedded script"

        ```html title="index.html"
        <script type="module">
          import { loadSpiceQL } from 'https://cdn.jsdelivr.net/npm/@usgs-astrogeology/spiceql/dist/spiceql.js';
          const spiceql = await loadSpiceQL();
          // ...
        </script>
        ```

## Examples

