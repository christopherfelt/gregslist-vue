<template>
  <div class="cars-component container">
    <div class="row">
      <div class="col">
        <button
          type="button"
          class="btn btn-success"
          v-if="!carForm"
          @click="carForm = !carForm"
        >
          Add Car
        </button>
        <button
          type="button"
          class="btn btn-danger"
          v-if="carForm"
          @click="
            carForm = !carForm;
            newCar = {};
          "
        >
          Cancel
        </button>
        <form class="form-inline" v-if="carForm" @submit.prevent="createCar">
          <form-comp
            v-for="input in carFormInputs"
            :key="input.name"
            :formInputProp="input"
            v-on:childToParent="updateParentData"
          />
          <button type="submit" class="btn btn-outline-success">Submit</button>
        </form>
      </div>
    </div>
    <div class="row">
      <car v-for="car in cars" :key="car._id" :carProp="car" />
    </div>
  </div>
</template>

<script>
import Car from "./CarComponent.vue";
import FormComp from "./FormComponent.vue";

export default {
  name: "Cars",
  // NOTE mounted fires at the first startup of the template being added to the page
  mounted() {
    this.$store.dispatch("getAllCars");
  },
  data() {
    return {
      newCar: {},
      carForm: false,
      carFormInputs: [
        { name: "Make", type: "text" },
        { name: "Model", type: "text" },
        { name: "Price", type: "number" },
        { name: "Year", type: "number" },
        { name: "Description", type: "text" },
        { name: "imgUrl", type: "text" },
      ],
      fromChild: "",
    };
  },
  computed: {
    cars() {
      return this.$store.state.cars;
    },
  },
  methods: {
    createCar() {
      this.$store.dispatch("createCar", { ...this.newCar });
      this.newCar = {};
    },
    updateParentData(childInput) {
      const fromChild = childInput;
      let childInputType =
        childInput.type == "number"
          ? parseInt(childInput.input)
          : childInput.input;
      if (childInput.name != "imgUrl") {
        this.newCar[childInput.name.toLowerCase()] = childInputType;
      } else {
        this.newCar[childInput.name] = childInputType;
      }
    },
  },
  components: {
    Car,
    FormComp,
  },
};
</script>

<style></style>
