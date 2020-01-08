<template>
  <div id="movie-list">
    <div v-if="filterMovies.length">
      <MovieItem v-for="movie in filterMovies" :movie="movie.movie" :key="movie.id" :sessions="movie.sessions" :day="day" :time="time" ></MovieItem>
    </div>
    <div v-else-if="movies.length" class="no-results" > No results for the selected combination!</div>
    <div v-else class="no-results"> Loading...</div>
  </div>
</template>

<script>
import genres from "../util/genres";
import MovieItem from "./MovieItem.vue";
import times from "../util/times"

export default {
  props: ["genre", "time", "movies", "day"],
  methods: {
    moviePassesGenreFilter(movie) {
      if (!this.genre.length) {
        return true;
      } else {
        let movieGenres = movie.movie.Genre.split(", ");
        let matched = true;
        this.genre.forEach(genre => {
          if (movieGenres.indexOf(genre) === -1) {
            matched = false;
          }
        });
        return matched;
      }
    },
    sessionPassesTimeFilter(session){
      if(!this.day.isSame(this.$moment(session.time), 'day')){
        return false
      } else if( this.time.length === 0 || this.time.length ===2 ){
        return true
      } else if( this.time[0] === times.AFTER_6PM ){
        return this.$moment(session.time).hour() >= 18
      } else {
        return this.$moment(session.time).hour() < 18
      }
    }
  },
  computed: {
    filterMovies() {
      return this.movies
      .filter(this.moviePassesGenreFilter)
      .filter(movie => movie.sessions.find(this.sessionPassesTimeFilter))
    }
  },
  components: {
    MovieItem
  },
  // data() {
  //   return {
  //     movies: [
  //       { title: "Kadambha", genre: genres.CRIME, id: 1 },
  //       { title: "Kotigobba", genre: genres.DRAMA, id: 2 },
  //       { title: "KGF Chapter 1", genre: genres.DOCUMENTARY, id: 3 }
  //     ]
  //   };
  // },
};
</script>