<script lang="tsx">
import { defineComponent, ref } from 'vue';
import env from '@/env';
import { Movie } from '@/types/Movie';

export default defineComponent({
  name: 'Home',
  setup() {
    const searchInput = ref<string>('');
    const movies = ref<Movie[]>([]);
    const handleSearchSubmit = (e: Event) => {
      e.preventDefault();
      if (searchInput.value !== '') {
        fetch(`http://www.omdbapi.com/?apikey=${env.apiKey}&s=${searchInput.value}`).then((res) => res.json()).then(({ Search }: {Search: Movie[]}) => {
          movies.value = Search;
          searchInput.value = '';
        });
      }
    };
    return () => (
      <div class="home">
        <div class="feature-card">
          <router-link to="/movie/tt0409591">
            <img src="https://m.media-amazon.com/images/M/MV5BZmQ5NGFiNWEtMmMyMC00MDdiLTg4YjktOGY5Yzc2MDUxMTE1XkEyXkFqcGdeQXVyNTA4NzY1MzY@._V1_SX300.jpg" alt="Naruto Poster" class="featured-img" />
            <div class="detail">
              <h3>Naruto</h3>
              <p>
                Naruto Uzumaki, a mischievous adolescent ninja,
                struggles as he searches for recognition and dreams of becoming the Hokage,
                the village's leader and strongest ninja.
              </p>
            </div>
          </router-link>
        </div>
        <form onSubmit={handleSearchSubmit} class="search-box">
          <input type="search" v-model={searchInput.value} placeholder="What are you looking for?..." />
          <input type="submit" value="Search" />
        </form>
        <div class="movie-list">
          {movies.value.map((movie: Movie) => (
            <div class="movie" key={movie.imdbID}>
              <router-link
                to={`/movie/${movie.imdbID}`}
                class="movie-link"
              >
                <div class="product-image">
                  <img src={movie.Poster} alt="Movie Poster" />
                  <div class="type">{movie.Type}</div>
                </div>
                  <div class="detail">
                    <p class="year">{movie.Year}</p>
                    <h3>{movie.Title}</h3>
                </div>
              </router-link>
            </div>
          ))}
        </div>
      </div>
    );
  },
});
</script>
<style lang="scss">
  .home {
    .feature-card {
      position: relative;
      .featured-img {
        display: block;
        width:100%;
        height: 300px;
        object-fit: cover;
        position: relative;
        z-index: 0;
      }
      .detail {
        position: absolute;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(0,0,0,0.6);
        padding: 16px;
        z-index: 1;
        h3 {
          color: #FFF;
          margin-bottom: 16px;;
        }
        p {
          color:#FFF;
        }
      }
    }
    .search-box {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      padding: 16px;

      input {
        display: block;
        appearance: none;
        border: none;
        outline: none;
        background: none;
        &[type="search"] {
          width: 100%;
          color: #FFF;
          background-color: #496583;
          font-size: 20px;
          padding: 10px 16px;
          border-radius: 8px;
          margin-bottom: 15px;
          transition: 0.4s;

          &::placeholder {
            color: #f3f3f3f3;
          }
          &:focus {
            box-shadow: 0px 3px 6px rgba(0,0,0, 0.2);
          }
        }
        &[type="submit"] {
          width: 100%;
          max-width: 300px;
          background-color: #42B883;
          padding: 16px;
          border-radius: 8px;
          color: #FFF;
          font-size: 20px;
          text-transform: uppercase;
          transition: 0.4s;
          &:active {
            background-color: #3BB070;
          }
        }
      }
    }
    .movie-list {
      display: flex;
      flex-wrap: wrap;
      margin: 0px 8px;
      .movie {
        max-width: 50%;
        flex: 1 1 50%;
        padding: 16px 8px;
        .movie-link {
          display: flex;
          flex-direction: column;
          height: 100%;

          .product-image {
            position:relative;
            display: block;
            img {
              display: block;
              width: 100%;
              height: 275px;
              object-fit: cover;
            }
            .type {
              position: absolute;
              padding: 8px 16px;
              background-color: #42B883;
              color: #FFF;
              bottom: 16px;
              left: 0px;
              text-transform: capitalize;
            }
          }
          .detail {
            background-color: #496583;
            padding: 16px 8px;
            flex: 1 1 100%;
            border-radius: 0px 0px 8px 8px;

            .year {
              color: #AAA;
              font-size: 14px;
            }
            h3 {
              color: #FFF;
              font-weight: 600;
              font-size: 18px;
            }

          }
        }
      }
    }
  }
</style>
