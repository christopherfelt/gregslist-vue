<template>
  <div class="form-group ml-2">
    <label for="title">{{ formInputProp.name }}</label>
    <input
      :type="formInputProp.type"
      class="form-control"
      :name="formInputProp.name"
      id="title"
      aria-describedby="title"
      v-model="childInput"
      v-on:input="emitToParent"
      required
    />
  </div>
</template>

<script>
export default {
  name: "FormComp",
  props: ["formInputProp"],
  data() {
    return {
      childInput: null,
    };
  },
  methods: {
    emitToParent(event) {
      this.$emit("childToParent", {
        name: this.$props.formInputProp.name,
        type: this.$props.formInputProp.type,
        input: this.childInput,
      });
    },
  },
};
</script>

<style scoped></style>
