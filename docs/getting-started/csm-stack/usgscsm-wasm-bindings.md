# Using USGSCSM WASM Bindings

USGSCSM provides WebAssembly (WASM) bindings that allow you to use Community Sensor Model (CSM)-compliant sensor models directly in JavaScript/TypeScript environments, including web browsers and Node.js applications.

## Overview

The WASM bindings enable you to:

- Use USGSCSM sensor models in web applications
- Perform camera model operations in JavaScript/TypeScript
- Access framing camera, line scan camera, and SAR sensor models
- Load and process Image Support Data (ISD) files
- Convert between image and ground coordinates

!!! tip "Try the Interactive Demo"
    Want to see USGSCSM WASM in action? Check out our [interactive browser-based demo](/docs/how-to-guides/demos/usgscsm-wasm-demo/) that demonstrates all the core functionality. The demo includes the WASM files and works out of the box!

## Included Files

USGSCSM WASM bindings are distributed via [NPM](https://www.npmjs.com/package/@usgs-astrogeology/usgscsm) and [GitHub Releases](https://github.com/DOI-USGS/usgscsm/releases).  Each release includes three files:

1. **[`usgscsm.wasm`](https://github.com/DOI-USGS/usgscsm/releases/download/2.1.0/usgscsm.wasm)** - The WebAssembly binary (~942 KB)
2. **[`usgscsm.js`](https://github.com/DOI-USGS/usgscsm/releases/download/2.1.0/usgscsm.js)** - JavaScript wrapper/loader (~124 KB)
3. **[`usgscsm.d.ts`](https://github.com/DOI-USGS/usgscsm/releases/download/2.1.0/usgscsm.d.ts)** - TypeScript type definitions (~9.69 KB)

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

    2.  **Install USGSCSM**  

        ```sh
        # In your terminal in the project folder:
        npm install @usgs-astrogeology/usgscsm
        ```

    3.  **Import USGSCSM** in your javascript

        ```js title="yourscript.js"
        import usgscsm from "@usgs-astrogeology/usgscsm/"
        ```

    4.  **Write Code with USGSCSM**

        See [Javascript - Quick Example](#examples) below.

    5.  **Run A Server**
    
        If you're using Vite, by default, you should be able to run a local server with:  
        ```sh
        # In your terminal in the project folder:
        npm run dev
        ```

=== "jsDelivr (CDN)"

    [jsDelivr](https://www.jsdelivr.com) is a Content Delivery Network. It fetches USGSCSM from NPM (or GitHub) for a quick and easy import with no setup.  It may add a little bit of loading time though.

    ```js title="yourscript.js"
    import usgscsm from 'https://cdn.jsdelivr.net/npm/@usgs-astrogeology/usgscsm/dist/usgscsm.js'
    ```

=== "Manual Download"

    Download the above files from the 
    [release page](https://github.com/DOI-USGS/usgscsm/releases), 
    and place the files in your project directory.  (For this example, in a `scripts` folder.)

    ```js title="yourscript.js"
    import usgscsm from "./scripts/usgscsm.js"
    ```

=== "⚠️ GitHub"

    !!! warning "Cannot Load Directly from GitHub"
        GitHub has headers that prevent direct script use in browsers. You must download the files and serve them from your own web server or local project, or use a CDN like unpkg or jsdelivr.

??? warning "Running a server and using `<script type="module" ...>` are required."

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
        import USGSCSM from 'https://cdn.jsdelivr.net/npm/@usgs-astrogeology/usgscsm/dist/usgscsm.js';
          // ...
        ```

    === "HTML with embedded script"

        ```html title="index.html"
        <script type="module">
          import USGSCSM from 'https://cdn.jsdelivr.net/npm/@usgs-astrogeology/usgscsm/dist/usgscsm.js';
          // ...
        </script>
        ```

## Examples

Import and wait for the `usgscsm()` module to load, then create the model, load an ISD, and perform CSM operations.

!!! note "Using your own ISD"

    For these examples, `const isdJson = await fetch(...url...)` gets 
    [simpleFramerISD](https://github.com/DOI-USGS/usgscsm/blob/main/tests/data/simpleFramerISD.json) from the USGSCSM test files. 
    Try replacing that fetch URL with your own ISD.
    
    *Caveat: For USGSCSM 2.1, make sure to 
    [add an `image-identifier`](#working-with-image-support-data-isd).*

???+ example "Javascript - Quick Example"

    ```js
    import usgscsm from 'https://cdn.jsdelivr.net/npm/@usgs-astrogeology/usgscsm/dist/usgscsm.js'

    usgscsm().then(async Module => {
      const model = new Module.USGSCSMModel();
      
      // Load camera model from ISD
      const isdJson = await fetch('https://cdn.jsdelivr.net/gh/DOI-USGS/usgscsm/tests/data/simpleFramerISD.json').then(r => r.text());
      model.loadFromISD(isdJson, 'USGS_ASTRO_FRAME_SENSOR_MODEL');
      
      // Transform coordinates
      const ground = model.imageToGround(512, 1024, 0);
      console.log(`Ground: (${ground.x}, ${ground.y}, ${ground.z})`);
    });
    ```

??? example "Javascript - Detailed Example"

    ```js
    import usgscsm from 'https://cdn.jsdelivr.net/npm/@usgs-astrogeology/usgscsm/dist/usgscsm.js'

    // Load the USGSCSM WASM module
    usgscsm().then(async Module => {
      // Create a new camera model
      const model = new Module.USGSCSMModel();
      
      // Load from ISD JSON (fetch from file or API)
      const isdJson = await fetch('https://cdn.jsdelivr.net/gh/DOI-USGS/usgscsm/tests/data/simpleFramerISD.json').then(r => r.text());
      const loaded = model.loadFromISD(isdJson, 'USGS_ASTRO_FRAME_SENSOR_MODEL');
      
      if (!loaded) {
        console.error('Failed to load model');
        return;
      }
      
      // Convert image coordinates to ground
      const ground = model.imageToGround(512, 512, 0);
      if (ground) {
        console.log('ECEF coordinates:', ground);
        // Output: { x: ..., y: ..., z: ... }
      }
      
      // Convert ground coordinates back to image
      const image = model.groundToImage(ground.x, ground.y, ground.z);
      if (image) {
        console.log('Image coordinates:', image);
        // Output: { line: 512, sample: 512 }
      }
      
      // Get sensor position
      const position = model.getSensorPosition(512, 512);
      console.log('Camera position:', position);
      
      // Export model state for faster loading next time
      const state = model.getModelState();
      localStorage.setItem('cameraModel', state);
      
      // Later, load from saved state (much faster than ISD)
      const newModel = new Module.USGSCSMModel();
      newModel.loadFromState(state);
    }).catch(console.error);
    ```

!!! example "The [USGSCSM Interactive Demo →](../../how-to-guides/demos/usgscsm-wasm-demo.md) and its [Source Code →](https://github.com/DOI-USGS/asc-public-docs/blob/main/docs/how-to-guides/demos/usgscsm-wasm-demo.html)"

??? example "HTML with Embedded Script"

    ```html
    <!DOCTYPE html>
    <html>
    <head>
      <title>USGSCSM WASM Example</title>
    </head>
    <body>
      <script type="module">
        import USGSCSM from "https://cdn.jsdelivr.net/npm/@usgs-astrogeology/usgscsm/dist/usgscsm.js";

        const Module = await USGSCSM();
        const model = new Module.USGSCSMModel();
    
        // Load camera model from ISD
        const isdJson = await fetch('https://cdn.jsdelivr.net/gh/DOI-USGS/usgscsm/tests/data/simpleFramerISD.json').then(r => r.text());
        model.loadFromISD(isdJson, 'USGS_ASTRO_FRAME_SENSOR_MODEL');
    
        // Transform coordinates
        const ground = model.imageToGround(512, 1024, 0);
        console.log(`Ground: (${ground.x}, ${ground.y}, ${ground.z})`);
      </script>
    </body>
    </html>
    ```

!!! info "For TypeScript projects, include `usgscsm.d.ts` in your project for type definitions."

## Sensor Model Types

USGSCSM provides three CSM-compliant sensor models through the WASM bindings:

1. **USGS_ASTRO_FRAME_SENSOR_MODEL** - Generic framing camera model
2. **USGS_ASTRO_LINE_SCANNER_SENSOR_MODEL** - Generic line scan camera model
3. **USGS_ASTRO_SAR_SENSOR_MODEL** - Generic synthetic-aperture radar (SAR) model

## Working with Image Support Data (ISD)

USGSCSM uses JSON-formatted Image Support Data (ISD) files to define camera models. ISD files can be generated using:

- [ALE (Abstraction Library for Ephemerides)](https://github.com/DOI-USGS/ale) with SPICE kernels
- ISIS cubes with attached SPICE data

The ISD format is converted internally to an optimized model state for efficient camera operations.

!!! warning "Known Issue in USGSCSM v2.1"
    USGSCSM v2.1 requires an `image_identifier` field in the ISD, even though standard ISDs from ALE don't include this field. This is a known issue - the C++ plugin automatically adds this field, but the WASM bindings bypass that step.
    
    **Workaround:** Add `image_identifier` to your ISD before loading:
    ```javascript
    const isd = JSON.parse(isdJson);
    isd.image_identifier = "my_image_name";
    model.loadFromISD(JSON.stringify(isd), modelType);
    ```
    
    This will be fixed in a future version to match the plugin behavior.

### Example ISD Structure

??? quote "ISD excerpt"

    A typical ISD JSON file includes:

    ```json
    {
      "radii": {
        "semimajor": 3396190.0,
        "semiminor": 3376200.0
      },
      "sensor_position": {
        "positions": [[3000000, 0, 2000000]],
        "velocities": [[0, 3000, 0]],
        "unit": "m"
      },
      "image_lines": 1024,
      "image_samples": 1024,
      "focal_length": 350.0,
      "detector_center": {
        "line": 512,
        "sample": 512
      }
    }
    ```

For a minimal working example, see [`simpleFramerISD.json`](https://github.com/DOI-USGS/usgscsm/blob/main/tests/data/simpleFramerISD.json) from the USGSCSM test files. For a complete, valid example, use ALE to generate an ISD from real mission data.

## File Requirements

When using USGSCSM WASM bindings:

1. Keep `usgscsm.js` and `usgscsm.wasm` in the same directory
2. The JavaScript file will automatically load the WASM file from the same directory
3. For TypeScript projects, include `usgscsm.d.ts` for type definitions

!!! note "File Paths"
    The `usgscsm.js` file expects `usgscsm.wasm` to be in the same directory by default. If you need to load the WASM file from a different location, you may need to configure the path in your bundler or build system.

## Additional Resources

- [USGSCSM GitHub Repository](https://github.com/DOI-USGS/usgscsm)
- [USGSCSM Releases](https://github.com/DOI-USGS/usgscsm/releases) - Download WASM bindings
- [NPM Package](https://www.npmjs.com/package/@usgs-astrogeology/usgscsm)
- [ALE (for generating ISD files)](https://github.com/DOI-USGS/ale)

## Support

For issues, questions, or contributions related to USGSCSM WASM bindings:

- Open an issue on the [USGSCSM GitHub repository](https://github.com/DOI-USGS/usgscsm/issues)

## Version Information

WASM support was introduced in USGSCSM version 2.1.0 (released June 2026).

Check the [releases page](https://github.com/DOI-USGS/usgscsm/releases) for the latest version and release notes.
