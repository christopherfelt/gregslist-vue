<template>
  <div class="houses-component container">
    <div class="row">
      <div class="col">
        <button
          type="button"
          class="btn btn-success"
          v-if="!houseForm"
          @click="houseForm = !houseForm"
        >
          Add House
        </button>
        <button
          type="button"
          class="btn btn-danger"
          v-if="houseForm"
          @click="
            houseForm = !houseForm;
            newCar = {};
          "
        >
          Cancel
        </button>
        <form
          class="form-inline"
          v-if="houseForm"
          @submit.prevent="createHouse()"
        >
          <form-comp
            v-for="input in houseFormInputs"
            :key="input.name"
            :formInputProp="input"
            v-on:childToParent="updateParentData"
          />
          <button type="submit" class="btn btn-outline-success">submit</button>
        </form>
      </div>
    </div>
    <div class="row">
      <house v-for="house in houses" :key="house.id" :houseProp="house" />
    </div>
  </div>
</template>

<script>
import FormComp from "./FormComponent.vue";
import House from "./HouseComponent.vue";

export default {
  name: "Houses",
  mounted() {
    this.$store.dispatch("getAllHouses");
  },
  data() {
    return {
      houseForm: false,
      newHouse: {},
      houseFormInputs: [
        { name: "Bedrooms", type: "number" },
        { name: "Bathrooms", type: "number" },
        { name: "Levels", type: "number" },
        { name: "Year", type: "number" },
        { name: "Description", type: "text" },
        { name: "imgUrl", type: "text" },
        { name: "Price", type: "number" },
      ],
    };
  },
  computed: {
    houses() {
      return this.$store.state.houses;
    },
  },
  methods: {
    createHouse() {
      this.$store.dispatch("createHouse", { ...this.newHouse });
      this.newHouse = {};
    },
    updateParentData(childInput) {
      const fromChild = childInput;
      let childInputType =
        childInput.type == "number"
          ? parseInt(childInput.input)
          : childInput.input;
      if (childInput.name != "imgUrl") {
        this.newHouse[childInput.name.toLowerCase()] = childInputType;
      } else {
        this.newHouse[childInput.name] = childInputType;
      }
    },
  },
  components: {
    FormComp,
    House,
  },
};
</script>

<style scoped></style>
