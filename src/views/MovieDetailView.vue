<template>
    <div class="movie-detail">
        <h2 class="movie-detail-title">{{ movie.Title }} <span class="movie-detail-year">({{ movie.Year }})</span></h2>
        <img :src="movie.Poster" alt="Movie Poster" class="movie-detail-img">
        <p class="movie-detail-plot">{{ movie.Plot }}</p>
        <div class="movie-detail-info">Director : {{ movie.Director }}</div>
        <div class="movie-detail-info">Writer : {{ movie.Writer }}</div>
        <div class="movie-detail-info">Actors : {{ movie.Actors }}</div>
        <div class="movie-detail-info">Awards : {{ movie.Awards }}</div>
        <div class="movie-detail-info">Country : {{ movie.Country }}</div>
        <div class="movie-detail-info">Genre : {{ movie.Genre }}</div>
    </div>
</template>

<script>
import { ref, onBeforeMount } from 'vue';
import { useRoute } from 'vue-router';
import env from '@/env.js';

export default {
    setup() {
        const movie = ref({});
        const route = useRoute();

        onBeforeMount(() => {
            fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&i=${route.params.id}&plot=full`)
                .then( response => response.json())
                .then( data => {
                    movie.value = data;
                    console.log(movie.value)
                })
        });

        return {
            movie,
            env,
            route,
            onBeforeMount
        }
    }
}
</script>

<style>
.movie-detail {
    padding: 16px;
    color: #fff;
}
.movie-detail-title {
    font-size: 28px;
    font-weight: 600;
    margin-bottom: 16px;
}
.movie-detail-year {
    font-size: 18px;
}
.movie-detail-img {
    display: block;
    max-width: 300px;
    margin: 0 auto 16px;
}
.movie-detail-plot {
    font-size: 17px;
    margin-bottom: 16px;
}
.movie-detail-info {
    font-size: 18px;
}
</style>