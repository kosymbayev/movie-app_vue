<template>
  <div class="home">
    <div class="feature-card">
        <router-link to='/movie/tt0409591'>
            <img src="https://m.media-amazon.com/images/M/MV5BZmQ5NGFiNWEtMmMyMC00MDdiLTg4YjktOGY5Yzc2MDUxMTE1XkEyXkFqcGdeQXVyNTA4NzY1MzY@._V1_SX300.jpg" alt="Naruto Poster" class="feature-img">
            <div class="detail">
                <h3>Naruto</h3>
                <p>Naruto Uzumaki, a mischievous adolescent ninja, struggles as he searches for recognition and dreams of becoming the Hokage, the village's leader and strongest ninja.</p>
            </div>
        </router-link>
    </div>

    <form @submit.prevent='SearchMovies()' class="search-box">
        <input type="text" placeholder="What are you looking for?" v-model="search">
        <input type="submit" value="Search">
    </form>

    <div class="movies-list">
        <div class="movie" v-for='movie in movies' :key='movie.imdbID'>
            <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
                <div class="product-image">
                    <img :src="movie.Poster" alt="Movie Poster" />
                    <div class="type">{{ movie.Type }}</div>
                </div>
                <div class="detail">
                    <p class="year">{{ movie.Year }}</p>
                    <h3>{{ movie.Title }}</h3>
                </div>
            </router-link>
        </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';
import env from '@/env.js';

export default {
    name: 'HomeView',
    setup() {

        const search = ref('');
        const movies = ref([]);

        const SearchMovies = () => {
            if( search.value != '' ) {
                fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
                .then( response => response.json())
                .then( data => {
                    movies.value = data.Search;
                    //console.log(movies.value)
                    search.value = '';
                })
            }
        }

        return {
            search,
            movies,
            SearchMovies
        }
    }

}
</script>

<style>
/* feature */
.feature-card {
    position: relative;
}
.feature-card .feature-img {
    position: relative;
    z-index: 0;
    display: block;
    width: 100%;
    height: auto;
    object-fit: cover;
}
.feature-card .detail {
    background: rgba(0, 0, 0, 0.6);
    padding: 16px;
    z-index: 1;
}
.feature-card h3 {
    color: #fff;
    margin-bottom: 16px;
}
.feature-card p {
    color: #fff;
}
@media screen and (min-width: 401px) {
    .feature-card > a {
        display: flex;
    }
    .feature-card .feature-img {
        max-width: 300px;
    }
    .feature-card .detail {
        width: 100%;
    }
}
@media screen and (max-width: 400px) {
    .feature-card .detail {
        position: absolute;
        left: 0;
        right: 0;
        bottom: 0;
    }
}

/* search */
.search-box {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 16px;
}
.search-box input {
    display: block;
    appearance: none;
    border: none;
    outline: none;
    background: none;
}
.search-box input[type='text'] {
    width: 100%;
    color: #fff;
    background: #496583;
    font-size: 20px;
    padding: 10px 16px;
    border-radius: 8px;
    margin-bottom: 15px;
    transition: 0.4s;
}
.search-box input[type='text']::placeholder {
    color: #f3f3f3;
}
.search-box input[type='text']:focus {
    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.2);
}
.search-box input[type='submit'] {
    width: 100%;
    max-width: 300px;
    background: #42b883;
    padding: 16px;
    border-radius: 8px;
    color: #fff;
    font-size: 20px;
    text-transform: uppercase;
    transition: 0.4s;
    cursor: pointer;
}
.search-box input[type='submit']:active {
    background: #3b8070;
} 

/* movies */
.movies-list {
    position: relative;
    display: grid;
    grid-template-columns: repeat( auto-fill, minmax(200px, 1fr));
    margin: 0 8px;
}
.movies-list .movie {
    padding: 16px 8px;
}
.movies-list .movie-link {
    display: flex;
    flex-direction: column;
    height: 100%;
}
.movies-list .product-image {
    position: relative;
    display: block;
}
.movies-list img {
    display: block;
    width: 100%;
    object-fit: cover;
    aspect-ratio: 8/11;
}
.movies-list .type {
    position: absolute;
    padding: 8px 16px;
    background: #42b883;
    color: #fff;
    bottom: 16px;
    left: 0;
    text-transform: capitalize;
}
.movies-list .detail {
    background-color: #496583;
    padding: 16px 8px;
    flex: 1 1 100%;
    border-radius: 0 0 8px 8px;
}
.movies-list .year {
    color: #aaa;
    font-size: 14px;
}
.movies-list h3 {
    color: #fff;
    font-weight: 600;
    font-size: 18px;
}

</style>