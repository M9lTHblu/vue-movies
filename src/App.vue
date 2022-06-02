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
  <div class="main-container">
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

<style scoped lang="scss">

.main-container {
  height: 100vh;
  background-color: #f4f4f4;
}

.header {
  position: sticky;
  top: 0;
  z-index: 999;
  width: 100%;
  padding: 0 1rem;
  color: var(--text-light);
  box-shadow: 0 3px 10px #7f7f7f;
  background-color: #292627;
}

.container {
  width: 1280px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: auto 260px 40px 900px auto;
  background-color: #fff;
}


.categories {
  position: relative;
  grid-column: 2 / 5;
  grid-row: 1 / 2;
  padding: 20px 0;
  display: flex;
  justify-content: space-between;

  ul {
    list-style: none;
    display: flex;
    gap: 1rem;
  }

  li {
    :hover {
      color: black;
    }
  }

  a {
    text-decoration: none;
    font-size: 15px;
    font-weight: 500;
    color: #777;
  }

  &::after {
    content: "";
    position: absolute;
    bottom: 0;
    left: 0;
    height: 1px;
    width: 100%;
    //grid-column: 2 / 5;
    background-color: #00000014;
  }
}

// SEARCH
.search__container {
  grid-column: 4 / 5;
  display: flex;
  align-items: center;
  justify-self: end;
}

.search__form {
  width: 300px;
  display: flex;
}

.search__input {

  input {
    outline: none;
    padding: .5rem;
    border: none;
    color: #A6A1A1FF;
    background-color: transparent;
  }

  width: 100%;
  border-radius: 4px;
  background-color: #474343;
  display: flex;
  justify-content: space-between;
}

.search__btn {
  width: 40px;
  height: 100%;
  padding: .5rem;
  outline: none;
  border: none;
  background-color: transparent;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

.headline {
  grid-column: 2 / 4;
  flex-grow: 1;
  color: var(--light);
  font-size: 2.5rem;
  font-weight: var(--bold);
  line-height: 2;
  letter-spacing: 2px;
}

// filters ----------------

.sidebar {
  height: 521px;
  grid-column: 2 / 3;
  grid-row: 3 / 4;
  position: sticky;
  top: 100px;
}

.sort {
  padding: 30px 16px 0 20px;
  grid-row: 2 / 3;
  grid-column: 4 / 5;
  display: flex;
  justify-content: space-between;

  .links {

    display: flex;
    gap: 1.5rem;
  }


  .link {
    color: #0006;
    text-decoration: none;
    display: flex;
    flex-direction: column;

    :first-child {
      font-size: 24px;
      line-height: 30px;
    }

    :last-child {
      font-size: 13px;
    }

    &:hover, .active {
      color: black
    }
  }
}

.dropdown__btn {
  display: flex;
  align-items: center;
  gap: .3rem;
  border: none;
  font-size: 15px;
  color: #333;
  background-color: transparent;
  cursor: pointer;
}

.dropdown__btn-icon {
  width: 0;
  height: 0;
  border-left: 5px solid transparent;
  border-right: 5px solid transparent;
  border-bottom: 8px solid grey;

  &.active {
    transform: rotate(180deg);
  }
}

// movies -----------------
.movies {
  grid-column: 4 / 5;
  overflow: hidden;
}

.movie {
  position: relative;
  padding: 1.5rem 1.25rem;
  cursor: pointer;
  display: flex;
  gap: 1.25rem;

  &::before {
    content: "";
    position: absolute;
    top: 0;
    width: 100%;
    height: 1px;
    background-color: #00000014;
  }

}

.movie__position {
  min-width: 1.5rem;
  font-size: 1rem;
  display: flex;
  justify-content: center;
}

.movie__poster {
  position: relative;
  display: flex;

  &::after {
    content: "";
    position: absolute;
    bottom: 0;
    right: 0;
    display: block;
    width: 26px;
    height: 18px;
    border-radius: 0 0 4px;
    background-color: #ffffffe6;
    background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' width='14' height='14'%3e%3cpath d='M4 2v10l8-5z'/%3e%3c/svg%3e");
    background-repeat: no-repeat;
    background-position: center;
  }
}

.movie__img {
  width: 64px;
  height: 96px;
  border-radius: 4px;
  object-fit: cover;
}

.movie__rating {
  position: absolute;
  top: 5px;
  left: -5px;
  padding: .188rem .438rem;
  border-radius: 2px;
  color: var(--light);
  font-size: .688rem;
  background-color: #3bb33b;
}

.movie__info {
  display: flex;
  flex-flow: column nowrap;

  p {

  }

  .movie__info-title {
    font-size: 1.125rem;
    line-height: 1.15;
  }
}


.movie__info-crew {
  color: #777;
  font-size: .813rem;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

</style>

fullTitle
