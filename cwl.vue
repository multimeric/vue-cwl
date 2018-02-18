<template>
    <svg ref="svg" class="cwl-workflow"></svg>
</template>

<script>
    import "cwl-svg/src/assets/styles/themes/rabix-dark/theme.scss";
    import "cwl-svg/src/plugins/port-drag/theme.dark.scss";
    import "cwl-svg/src/plugins/selection/theme.dark.scss";

    import {WorkflowFactory} from "cwlts/models";
    import {Workflow, SVGArrangePlugin, SelectionPlugin} from "cwl-svg";

    export default {
        data() {
            return {
                selectedNode: null,
                workflow: null,
                cwlState: null
            };
        },

        computed: {
            cwlModel() {
                return WorkflowFactory.from(this.cwlState);
            }
        },

        props: {
            cwlUrl: {
                type: String,
                default: null,
                note: `A URL to request for the initial CWL object from. Used as an alternative to
                the "cwl" prop`
            },
            cwl: {
                type: Object,
                default: null,
                note: `The JSON object representing the CWL workflow to render`
            },

            editingEnabled: {
                type: Boolean,
                default: false,
                note: `True if the workflow is editable`
            },
            plugins: {
                type: Array,
                default: () => [],
                note: `A list of CWL plugins to use in the CWL rendering`
            }
        },

        /**
         * If the cwlUrl prop was set, send a request for the CWL object, and set it to the internal
         * state
         */
        mounted(){
            if (this.cwlUrl){
                fetch(this.cwlUrl, {
                    headers: new Headers({
                        'Accept': 'application/json'
                    })
                }).then(response => {
                    return response.json();
                }).then(json => {
                    this.cwlState = json;
                });
            }
        },

        watch: {
            /**
             * If the cwl prop ever changes, update the internal workflow object to that
             */
            cwl() {
                this.cwlState = this.cwl;
            },

            workflow(){
                this.$emit('workflow-changed', this.workflow)
            },

            cwlState(){
                this.workflow = new Workflow({
                    editingEnabled: this.editingEnabled,
                    model: this.cwlModel,
                    svgRoot: this.$refs.svg,
                    plugins: this.plugins
                });

                // Hack to force ArrangePlugin to rearrange
                const arranger = this.workflow.getPlugin(SVGArrangePlugin);
                if (arranger)
                    arranger.arrange();

                // Emit a selectionChanged event when selection changes
                const selection = this.workflow.getPlugin(SelectionPlugin);
                selection.registerOnSelectionChange(element => {
                    if (element) {
                        const id = element.getAttribute("data-connection-id");
                        const selected = this.workflow.model.findById(id);
                        this.$emit('selection-changed', selected);
                    }
                });
            }
        }
    }
</script>

<style lang="css">
    .cwl-workflow {
        height: 500px;
        position: relative;
    }
</style>