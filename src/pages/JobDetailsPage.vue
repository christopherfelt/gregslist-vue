<template>
  <div class="job-details container">
    <div class="row">
      <div class="col-6">
        <h3>Company: {{ job.company }}</h3>
        <h3>Job Title: {{ job.jobTitle }}</h3>
        <h3>Rate: {{ job.rate }}</h3>
        <h3>Hours: {{ job.hours }}</h3>
        <button type="button" class="btn btn-outline-danger" @click="deleteJob">
          DELETE!
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "JobDetails",
  mounted() {
    this.$store.dispatch("getJobDetails", this.$route.params.id);
  },

  computed: {
    job() {
      return this.$store.state.activeJob;
    },
  },
  methods: {
    deleteJob() {
      this.$store.dispatch("deleteJob", this.job._id);
    },
  },
  beforeRouteLeave(to, from, next) {
    this.$store.commit("setActiveJob", {});
    next();
  },
};
</script>

<style scoped></style>
