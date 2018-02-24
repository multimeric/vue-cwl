# Vue CWL
Vue CWL provides a Vue component for the visualization of CWL (Common Workflow Language) workflows.

## Installation
Install from `npm`:

```bash
npm install vue-cwl --save-dev
```

Or, if you want the latest development version, install from github:

```bash
npm install git+https://git@github.com/TMiguelT/vue-cwl.git --save-dev
```

## Adding to Vue
Vue CWL is exported as a UMD module, which means that you can either import it with a module loader,
or import it directly in the browser.

If using webpack:

```javascript
import Vue from 'vue';
import Cwl from 'cwl-svg';

new Vue({
    el: '#vue',
    components: {
        cwl: Cwl
    }
})
```

If using no bundler:

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.13/dist/vue.js"></script>
<script src="node_modules/vue-cwl/dist/index.js"></script>
<script>
    Vue.config.devtools = true;
    Vue.config.debug = true;
    new Vue({
        el: '#vue',
        components: {
            cwl: vueCwl.default
        }
    })
</script>
```

## Usage

Vue CWL components currently accept 4 props:

* cwl:
    type: Object
    default: null
    description: The JSON object representing the CWL workflow to render
* cwlUrl:
    type: String
    default: null
    description: A URL to request for the initial CWL object from. Used as an alternative to the "cwl" prop`
* editingEnabled:
    type: Boolean
    default: false
    description: True if the workflow is editable
* plugins:
    type: Array
    default: No plugins
    description: A list of CWL plugins to use in the CWL rendering

## Demo
If you wish to see a working example of Vue CWL, have a look at then demo.

[The source code is available here](demo/index.html)

To run the demo:

* Clone the repo
* Run `npm run demo`
* Then browse to <http://localhost:8080/demo/> in your web browser (or whatever URL the command
prints out)
