<template>
  <MovieList @onmessage="handleMessage">
    <MovieItem v-for="movie in movieList" :key="movie.id" :movie="movie" />
  </MovieList>
</template>

<script>
import http from "utils/http";
import _ from 'lodash'
import MovieItem from "components/movie_list/MovieItem";
import MovieList from "components/movie_list/MovieList";
export default {
  components: {
    MovieList,
    MovieItem
  },
  beforeCreate() {
        this.page = 0
        this.movieIds = []
        this.chunkedMovieIds = []
        this.limit = 12
    },
  data() {
    return {
      movieList: []
    };
  },

  async created() {
    let result = await http.get({ url: "/ajax/movieOnInfoList?token=" });
    this.movieList = result.movieList;
    this.movieIds = result.movieIds.slice(this.limit)
    this.chunkedMovieIds = _.chunk(this.movieIds, this.limit)
  },
  methods:{
    async handleMessage(bscroll){
      let result = await http.get({url:'/ajax/moreComingList?ci=1&token=&limit='+this.limit+'&movieIds='+this.chunkedMovieIds[this.page].join(',')})
      this.movieList=[
        ...this.movieList,
        ...result.coming
      ]
      this.$nextTick(() => {
        bscroll.refresh()
        bscroll.finishPullUp()
        this.page++
      })
    }
  }
};
</script>

<style lang="stylus" scoped></style>