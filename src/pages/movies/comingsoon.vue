<template>
   <MovieList @onmessage="handleMessage">
       <MostExpected></MostExpected>
       <div class="coming-list">
            <div v-for="(list,key) in comingList" :key="key">
                <p class='group-date'>{{key}}</p>
                <MovieItem v-for="movie in list" :key="movie.id" :movie="movie" />
            </div>
    </div>
   </MovieList>
</template>

<script>
import http from 'utils/http'
import _ from 'lodash'
import MovieList from 'components/movie_list/MovieList'
import MovieItem from 'components/movie_list/MovieItem'
import MostExpected from './MostExpected'
export default {
    data(){
        return{
            movieList:[]
        }
    },
    beforeCreate() {
        this.page = 0
        this.movieIds = []
        this.chunkedMovieIds = []
        this.limit = 10
    },
    components:{
        MovieList,
        MovieItem,
        MostExpected
    },
    computed: {
        comingList() {
            let groupedList = _.groupBy(this.movieList, (value) => {
                return value.comingTitle
            })
            return groupedList
        }
    },

    async created() {
        let result = await http.get({
        url: "/ajax/comingList?ci=1&token=&limit=" + this.limit
        })
        this.movieList = result.coming
        this.movieIds = result.movieIds.slice(this.limit)
        this.chunkedMovieIds = _.chunk(this.movieIds, this.limit)
    },
    methods: {
        async handleMessage(bScroll) {
            let result = await http.get({
                url: "/ajax/moreComingList?ci=1&token=&limit=" + this.limit + "&movieIds=" + this.chunkedMovieIds[this.page].join(',')
            })

            this.movieList = [
                ...this.movieList,
                ...result.coming
            ]

            this.$nextTick(() => {
                bScroll.refresh()
                bScroll.finishPullUp()
                this.page++
            })
        }
    },
}
</script>

<style lang="stylus" scoped>

</style>