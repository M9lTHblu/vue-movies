<script>

export default {
  name: 'App',
  data() {
    return {
      movies: null,
      error: null,
      selectYear: null,
    };
  },
  methods: {
    fetchData() {
      fetch('https://imdb-api.com/en/API/MostPopularMovies/k_v9sqhx81').
          then((res) => res.json()).
          then(json => this.movies = json.items).
          catch(e => this.error = e);
    },
  },
  mounted() {
    console.count('fetch');
    this.fetchData();
  },

};
</script>


<template>
  <div class="main-container" v-cloak>
    <!-- HEADER -->
    <header class="header">
      <div class="container" :style="{backgroundColor: 'transparent'}">
        <h1 class="headline">Vue Movies</h1>
        <!-- SEARCH -->
        <div class="search__container">
          <form class="search__form">
            <div class="search__input">
            <input  type="text" placeholder="Search movie"/>
              <button class="search__btn">
                <svg xmlns="http://www.w3.org/2000/svg"  viewBox="0 0 20 20"  fill="#9c9595">
                  <path d="M0 0h24v24H0V0z" fill="none" />
                  <path
                      d="M15.5 14h-.79l-.28-.27C15.41 12.59 16 11.11 16 9.5 16 5.91 13.09 3 9.5 3S3 5.91 3 9.5 5.91 16 9.5 16c1.61 0 3.09-.59 4.23-1.57l.27.28v.79l5 4.99L20.49 19l-4.99-5zm-6 0C7.01 14 5 11.99 5 9.5S7.01 5 9.5 5 14 7.01 14 9.5 11.99 14 9.5 14z" />
                </svg>
              </button>
            </div>
          </form>
        </div>
      </div>
    </header>
    <div class="container" :style="{marginTop: '2rem'}">
      <!-- CATEGORIES  -->
      <div class="categories">
        <a href="#">
          All lists
        </a>
        <ul>
          <li><a href="#">Top 250 Movies</a></li>
          <li><a href="#">Top 250 TVs</a></li>
          <li><a href="#">Most Popular Movies</a></li>
          <li><a href="#">Most Popular TVs</a></li>
        </ul>
      </div>
      <!-- SIDEBAR -->
      <aside class="sidebar">

      </aside>
      <!-- SORT-DROPDOWN -->
      <div class="sort">
        <div class="links">
          <a class="link" href="#">
            <span>Online</span>
            <span>movies</span>
          </a>
          <a class="link" href="#">
            <span>All</span>
            <span>movies</span>
          </a>
        </div>
        <div class="dropdown">
          <button class="dropdown__btn">
            <span>In order</span>
            <span class="dropdown__btn-icon"></span>
          </button>
          <div class="dropdown__menu" :style="{display: 'none'}">
            <span></span>
            <span>By rating</span>
            <span>By release date</span>
            <span>By name</span>
          </div>
        </div>
      </div>
      <!-- MOVIES -->
      <div class="movies">
        <div v-if="error">{{ error.message }}}</div>
        <article class="movie"
                 v-else-if="movies"
                 v-for="movie in movies"
                 :key="movie.id"
        >
          <div class="movie__position">
            <span>{{ movie.rank }}</span>
          </div>
          <a class="movie__poster">
            <img class="movie__img" :src="movie.image" :alt="movie.title">
            <span class="movie__rating">{{ movie.imDbRating }}</span>
          </a>

          <div class="movie__info">
            <span class="movie__info-title">{{ movie.title }}</span>
            <span :style="{fontSize: '13px'}">{{ movie.title }}, {{ movie.year }}</span>
            <div class="movie__info-crew">
              Film crew:
              <span>{{ movie.crew }}</span>
            </div>
          </div>
        </article>
        <div v-else>Loading...</div>
      </div>
    </div>
  </div>

</template>

