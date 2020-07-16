<template>
  <div ref="root">
    <component
      v-bind:is="component"
      class="tab"
      v-bind:document="document"
      v-bind:raw-document="document"
      v-bind:handle-obfuscation="handleObfuscation"
    />
  </div>
</template>

<script>
import penpal from "penpal";
import DefaultTemplate from "../core/DefaultTemplate.vue";
import { markRaw } from "@vue/reactivity";
const defaultTemplate = [
  {
    id: "default-template",
    label: "Default",
    template: DefaultTemplate,
  },
];

export default {
  name: "FramedDocumentRenderer",
  props: {
    templateRegistry: Object,
  },
  mounted() {
    const dispatch = ({ type, payload }) => {
      if (type === "RENDER_DOCUMENT") {
        this.document = payload.document;
        this.rawDocument = payload.rawDocument;

        const templates = this.getTemplates(this.document);
        this.selectedTemplate = templates[0].id;

        this.host.dispatch({ type: "UPDATE_TEMPLATES", payload: templates.map(({ id, label }) => ({ id, label })) });
      } else if (type === "PRINT") {
        window.print();
      } else if (type === "SELECT_TEMPLATE") {
        this.selectedTemplate = payload;
      } else {
        console.error(`Unable to handle ${type}`);
      }
    };
    penpal.connectToParent({ methods: { dispatch } }).promise.then((host) => {
      this.host = host;

      this.observer = new MutationObserver(() => {
        this.updateHeight();
      });
      this.observer.observe(this.$refs.root, { attributes: true, childList: true, subtree: true, characterData: true });
      this.updateHeight();
    });
  },
  beforeDestroy() {
    if (this.observer) {
      this.observer.disconnect();
    }
  },
  data() {
    return {
      selectedTemplate: null,
      document: null,
      rawDocument: null,
      host: null,
      observer: null,
    };
  },
  computed: {
    component() {
      if (this.document) {
        const templates = this.getTemplates(this.document);
        return templates.find((template) => template.id === this.selectedTemplate).template;
      }
      return null;
    },
  },
  methods: {
    getTemplates(document) {
      return this.templateRegistry[document.$template.name] || defaultTemplate;
    },
    handleObfuscation(field) {
      this.host.dispatch({ type: "OBFUSCATE", payload: field });
    },
    updateHeight() {
      // using https://stackoverflow.com/questions/1145850/how-to-get-height-of-entire-document-with-javascript
      const body = window.document.body,
        html = window.document.documentElement;

      const height = Math.max(
        body.scrollHeight,
        body.offsetHeight,
        html.clientHeight,
        html.scrollHeight,
        html.offsetHeight
      );
      console.log("update height", height);
      this.host.dispatch({ type: "UPDATE_HEIGHT", payload: height });
    },
  },
};
</script>

<style scoped></style>
