<template>
  <div class="movie-info-wrap">
    <header class="movie-info-header">
      <h6 class="movie-header-title">Movie view</h6>
      <BIconX class="close-icon" @click="closeModal" />
    </header>
    <div class="movie-info-content">
      <BRow>
        <BCol sm="4">
          <div class="movie-poster-wrap">
            <div class="movie-poster" :style="posterStyle"></div>
          </div>
        </BCol>
        <BCol sm="8">
          <h3 class="movie-title">{{ movie.Title }}</h3>
          <BFormRating
            class="movie-rating"
            color="gold"
            v-model="movie.imdbRating"
            readonly
            show-value
            precision="1"
            stars="10"
            show-value-max
            no-border
          />
          <p class="movie-description">{{ movie.Plot }}</p>
          <div>
            <BBadge class="movie-badge mr-2">{{ movie.Year }}</BBadge>
            <BBadge class="movie-badge mr-2">{{ movie.Runtime }}</BBadge>
            <BBadge class="movie-badge mr-2">{{ movie.Genre }}</BBadge>
            <BBadge class="movie-badge mr-2">{{ movie.Language }}</BBadge>
          </div>
          <table class="table small">
            <tbody>
              <tr>
                <th>Production</th>
                <td>{{ movie.Production }}</td>
              </tr>
              <tr>
                <th>Country</th>
                <td>{{ movie.Country }}</td>
              </tr>
              <tr>
                <th>Director</th>
                <td>{{ movie.Director }}</td>
              </tr>
              <tr>
                <th>Writer</th>
                <td>{{ movie.Writer }}</td>
              </tr>
              <tr>
                <th>Actors</th>
                <td>{{ movie.Actors }}</td>
              </tr>
              <tr>
                <th>Awards</th>
                <td>{{ movie.Awards }}</td>
              </tr>
            </tbody>
          </table>
        </BCol>
      </BRow>
    </div>
  </div>
</template>

<script>
export default {
  name: "ModalInfoContent",
  props: {
    movie: {
      type: Object,
      requred: true,
    },
  },
  data: () => ({
    defaultPosterBg:
      "linear-gradient(45deg, rgba(0, 3, 38) 0%, rgba(82, 15, 117) 100%);",
  }),
  computed: {
    posterStyle() {
      return {
        // eslint-disable-next-line
        "background-image": this.posterBg,
      };
    },
    posterBg() {
      return this.movie ? `url(${this.movie.Poster})` : this.defaultPosterBg;
    },
  },
  methods: {
    closeModal() {
      this.$emit("closeModal");
    },
  },
};
</script>

<style scoped>
.movie-info-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  background: linear-gradient(45deg, rgb(0, 3, 38) 0%, rgb(82, 15, 117) 100%);
  color: #fff;
}
.movie-header-title {
  margin-bottom: 0;
  line-height: 1.5;
  font-size: 1.25rem;
}

.movie-info-content {
  padding: 1rem;
  background-color: #fff;
}

.movie-poster-wrap {
  position: relative;
  padding-bottom: 150%;
  background-repeat: 5px;
  overflow: hidden;
  transition: all 0.2s ease;
}

.movie-poster {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-size: cover;
  background-position: center center;
}

.movie-title {
  font-size: 3.5rem;
  font-weight: 300;
  line-height: 1.2;
}

.movie-rating:focus {
  box-shadow: none;
}

.movie-rating {
  padding: 0;
}

.movie-rating >>> .b-rating-star,
.movie-rating >>> .b-rating-value {
  justify-content: flex-start;
  flex-grow: 0 !important;
  font-size: 1.3rem;
  font-weight: 300;
  padding: 0;
}

.movie-rating >>> .b-rating-star + .b-rating-star {
  margin-left: 4px;
}

.movie-rating >>> .b-rating-value {
  margin-left: 10px;
}

.movie-description {
  font-size: 1.25rem;
  font-weight: 300;
}

.movie-badge {
  color: #fff;
  background-color: rgb(62, 163, 62);
  margin-right: 2rem;
  margin-bottom: 1rem;
}

.close-icon {
  font-size: 24px;
  cursor: pointer;
}

@media (max-width: 992px) {
  .movie-rating >>> .b-rating-star,
  .movie-rating >>> .b-rating-value {
    font-size: 1rem;
    font-weight: 200;
    padding: 0;
  }
  .movie-title {
    font-size: 2.4rem;
    font-weight: 300;
  }
  .movie-description {
    font-size: 1rem;
  }
}
</style>
