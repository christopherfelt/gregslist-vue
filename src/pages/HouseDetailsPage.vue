<template>
  <div class="house-details container">
    <div class="row">
      <div class="col">
        <h1>Price: {{ house.price }}</h1>
        <h1>Bathrooms: {{ house.bathrooms }}</h1>
        <h1>Bedrooms: {{ house.bedrooms }}</h1>
        <h1>Levels: {{ house.levles }}</h1>
        <h1>Year: {{ house.year }}</h1>
        <h1>Description: {{ house.description }}</h1>
        <button
          type="button"
          class="btn btn-outline-danger"
          @click="deleteHouse"
        >
          Delete
        </button>
      </div>
      <div class="col">
        <img class="card-img-top" :src="house.imgUrl" alt="" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HouseDetails",
  mounted() {
    this.$store.dispatch("getHouseDetails", this.$route.params.id);
  },
  computed: {
    house() {
      return this.$store.state.activeHouse;
    },
  },
  methods: {
    deleteHouse() {
      this.$store.dispatch("deleteHouse", this.house._id);
    },
  },
  beforeRouteLeave(to, from, next) {
    this.$store.commit("setActiveHouse", {});
    next();
  },
};
</script>

<style scoped></style>
