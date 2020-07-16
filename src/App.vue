<template>
  <FramedDocumentRenderer v-bind:template-registry="templateRegistry" />
</template>

<script>
import FramedDocumentRenderer from "./components/core/FramedDocumentRenderer.vue";
import Template from "./components/Template.vue";

export default {
  name: "App",
  components: {
    FramedDocumentRenderer,
  },
  data() {
    return {
      templateRegistry: {
        GOVTECH_DEMO: [
          {
            id: "demo",
            label: "Demo",
            template: Template,
          },
        ],
      },
    };
  },
};
</script>
