<script lang="tsx">
import { defineComponent, onBeforeMount, ref } from 'vue';
import { useRoute } from 'vue-router';
import { Movie, MovieDetail } from '@/types/Movie';
import env from '@/env';

type FullMovieDetail = Movie & MovieDetail;
export default defineComponent({
  setup() {
    const route = useRoute();
    const movie = ref<Partial<Movie & MovieDetail>>({});
    onBeforeMount(() => {
      fetch(`http://www.omdbapi.com/?apikey=${env.apiKey}&i=${route.params.id}&plot=full`)
        .then((res) => res.json())
        .then((data: FullMovieDetail) => {
          movie.value = data;
        });
    });
    return () => (
      <div class="movie-detail">
        <h2>{movie.value.Title}</h2>
        <p>{movie.value.Year}</p>
        <img src={movie.value.Poster} alt="Movie Poster" class="featured-image" />
        <p>{movie.value.Plot}</p>
      </div>
    );
  },
});
</script>
<style lang="scss">
  .movie-detail {
    padding: 16px;

    h2 {
      color: #FFF;
      font-size: 28px;
      font-weight:600;
      margin-bottom: 16px;
    }
    .featured-image {
      display: block;
      max-width: 200px;
      margin-bottom: 16px;
    }
    p {
      color: #FFF;
      font-size: 18px;
      line-height: 1.4;
    }
  }
</style>
