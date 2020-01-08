<template>
  <div id="movie-filter">
    <h2>Filter results</h2>
    <h3>By time of day</h3>
    <div class="filter-group">
      <check-filter v-for="time in times" category="time" :title="time" :key="time"></check-filter>
    </div>
    <h3>By Genre</h3>
    <div class="filter-group">
      <check-filter v-for="genre in genres" category="genre" :title="genre" :key="genre"></check-filter>
    </div>
  </div>
</template>

<script>
import genres from "../util/genres";
import CheckFilter from "../components/CheckFilter.vue"
import times from "../util/times";

export default {
  data() {
    return {
      genres, times
    };
  },
  components: {
    CheckFilter
  }
};
</script>