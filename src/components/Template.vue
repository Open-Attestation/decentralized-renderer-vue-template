<template>
  <div class="main">
    <h1>Hello from vue</h1>
    <div>
      <button v-on:click="handleClick">
        Hide field id
      </button>
    </div>
    <div>id: {{ document.id }}</div>
    <div>issued on: {{ document.issuedOn }}</div>
    <div>description: {{ document.description }}</div>
  </div>
</template>

<script>
export default {
  name: "Template",
  props: {
    document: Object,
    rawDocument: Object,
    handleObfuscation: Function,
  },
  methods: {
    handleClick() {
      this.handleObfuscation("id");
    },
  },
};
</script>

<style scoped>
.main {
  background-color: #676778;
  color: white;
  text-align: center;
  padding: 1em;
  margin: 0 auto;
}
</style>
