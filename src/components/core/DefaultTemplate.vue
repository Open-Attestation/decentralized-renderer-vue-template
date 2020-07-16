<template>
  <div id="default-template">
    <h3>This is the OpenAttestation default renderer</h3>
    <div class="alert" role="alert">
      You see this template because the certificate issuer misconfigured the template configuration of your document.
      Please check with the certificate issuer. More information is available in
      <a href="https://openattestation.com/docs/advanced/custom-renderer" target="_blank" rel="noopener noreferrer">
        the documentation
      </a>
      .
    </div>
    <pre>{{ JSON.stringify(document, null, 2) }}</pre>
  </div>
</template>

<script>
export default {
  name: "DefaultTemplate",
  props: {
    document: Object,
    rawDocument: Object,
  },
};
</script>

<style scoped>
h3 {
  text-align: center;
}
.alert {
  text-align: center;
  margin-top: 1rem;
  margin-bottom: 1rem;
  color: #856404;
  background-color: #fff3cd;
  border: 1px solid #ffeeba;
  padding: 0.75rem 1.25rem;
}
pre {
  background-color: lightgray;
}
</style>
