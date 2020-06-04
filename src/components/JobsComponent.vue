<template>
  <div class="jobs-component container">
    <div class="row">
      <div class="col">
        <button
          type="button"
          class="btn btn-success"
          v-if="!jobForm"
          @click="jobForm = !jobForm"
        >
          Add Jobs
        </button>
        <button
          type="button"
          class="btn btn-danger"
          v-if="jobForm"
          @click="jobForm = !jobForm"
        >
          Cancel
        </button>
        <form class="form-inline" v-if="jobForm" @submit.prevent="createJob()">
          <form-comp
            v-for="input in jobFormInputs"
            :key="input.name"
            :formInputProp="input"
            v-on:childToParent="updateParentData"
          />
          <button type="submit" class="btn btn-outline-success">Submit</button>
        </form>
      </div>
    </div>
    <div class="row">
      <job v-for="job in jobs" :key="job._id" :jobProp="job" />
    </div>
  </div>
</template>

<script>
import FormComp from "./FormComponent.vue";
import Job from "./JobComponent.vue";

export default {
  name: "Jobs",
  mounted() {
    this.$store.dispatch("getAllJobs");
  },
  data() {
    return {
      jobForm: false,
      jobFormInputs: [
        { name: "company", type: "text" },
        { name: "jobTitle", type: "text" },
        { name: "hours", type: "number" },
        { name: "rate", type: "number" },
        { name: "description", type: "text" },
      ],
      newJob: {},
    };
  },
  computed: {
    jobs() {
      return this.$store.state.jobs;
    },
  },
  methods: {
    createJob() {
      this.$store.dispatch("createJob", { ...this.newJob });
      this.newJob = {};
    },
    updateParentData(childInput) {
      const fromChild = childInput;
      let childInputType =
        childInput.type == "number"
          ? parseInt(childInput.input)
          : childInput.input;
      if (childInput.name != "jobTitle") {
        this.newJob[childInput.name.toLowerCase()] = childInputType;
      } else {
        this.newJob[childInput.name] = childInputType;
      }
    },
  },
  components: {
    FormComp,
    Job,
  },
};
</script>

<style scoped></style>
