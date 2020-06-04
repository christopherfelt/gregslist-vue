<template>
  <div class="job col-3">
    <router-link :to="{ name: 'JobDetails', params: { id: jobProp._id } }">
      <div class="card">
        <div class="card-body">
          <h4 class="card-title">{{ jobProp.company }}</h4>
        </div>
      </div>
    </router-link>
  </div>
</template>

<script>
export default {
  name: "job-component",
  props: ["jobProp"],
};
</script>

<style scoped></style>
