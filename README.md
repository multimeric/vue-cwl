# Vue CWL

[![DOI](https://zenodo.org/badge/121595617.svg)](https://zenodo.org/badge/latestdoi/121595617)

Vue CWL provides a Vue component for the visualization of CWL (Common Workflow Language) workflows.

## Motivation

Vue CWL uses the fantastic [cwl-svg](https://github.com/rabix/cwl-svg) library behind the scenes, which you can also use directly.
However, you might want to use Vue CWL because:

* You can use it directly in your HTML application, without setting up a compiler. You don't even need to use npm!
* If you're using Vue elsewhere in your application, Vue CWL will be much easier to integrate.

## Installation (if using npm)
Install from npm:

```bash
npm install vue-cwl --save-dev
```

Or, if you want the latest development version, install from github:

```bash
npm install git+https://git@github.com/TMiguelT/vue-cwl.git --save-dev
```

## Integration
Vue CWL is exported as a UMD module, which means that you can either import it with a module loader,
or import it directly in the browser.

If using webpack and npm:

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

You can now use the `<cwl>` tag in your template:
```html
<cwl
    cwl-url="https://unpkg.com/cwl-svg@2.1.5/cwl-samples/fastqc.json"
></cwl>
```

If using no bundler, you can include Vue and Vue CWL using something like `unpkg`:

```html
<div id="vue" style="width: 800px; height: 800px">
    <cwl
            cwl-url="https://unpkg.com/cwl-svg@2.1.5/cwl-samples/fastqc.json"
    ></cwl>
</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.6.10/vue.min.js"></script>
<script src="https://unpkg.com/vue-cwl/dist/index.js"></script>
<script>
    new Vue({
        el: '#vue',
        components: {
            cwl: vueCwl.default
        }
    });
</script>
```

## Usage

Vue CWL components currently accept 4 props:

* `cwl`:
    * type: Object
    * default: null
    * description: The JSON object representing the CWL workflow to render
* `cwlUrl`:
    * type: String
    * default: null
    * description: A URL to request for the initial CWL object from. Used as an alternative to the "cwl" prop`
* `editingEnabled`:
    * type: Boolean
    * default: false
    * description: True if the workflow is editable
* `plugins`:
    * type: Array
    * default: No plugins
    * description: A list of CWL plugins to use in the CWL rendering

## Demo
An online demo of Vue CWL is available at <https://multimeric.github.io/vue-cwl/>

If you wish to build the demo yourself:
* Clone the repo
* Run `npm run demo`
* Then browse to <http://localhost:8080/> in your web browser (or whatever URL the command
prints out)
