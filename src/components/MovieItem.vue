<template>
  <div class="movie-item mb-3">
    <div class="movie-item-poster" :style="posterBg"></div>
    <div class="movie-info-wrap d-flex flex-column justify-content-between">
      <div class="movie-item-info">
        <h3 class="movie-title">
          {{ movieId + "." }} {{ movie.Title }} ({{ movie.Year }})
        </h3>
        <div class="movies-rating">
          <div class="movie-rating">
            <span>IMDB: </span>
            <span>{{ movie.Ratings[0].Value }}</span>
          </div>
          <div class="movie-rating">
            <span>Tomatoes: </span>
            <span>{{ movie.Ratings[1].Value }}</span>
          </div>
        </div>
      </div>
      <div class="movie-item-controls row no-gutters">
        <div class="col pr-2">
          <BButton
            class="movie-buttons"
            block
            size="md"
            variant="outline-light"
            @click="showInfoModalEvent"
            >Info</BButton
          >
        </div>
        <div class="col pl-2">
          <BButton
            class="movie-buttons"
            block
            size="md"
            variant="outline-light"
            @click="emitRemoveEvent"
            >Remove</BButton
          >
        </div>
      </div>
    </div>
    <div class="movie-bottom-info">
      <h3 class="movie-bottom-title">
        {{ movieId + "." }} {{ movie.Title }} ({{ movie.Year }})
      </h3>
    </div>
  </div>
</template>

<script>
export default {
  name: "MovieItem",
  props: {
    movie: {
      type: Object,
      required: true,
    },
    id: {
      type: Number,
      required: true,
    },
    currentPage: {
      type: Number,
      required: true,
    },
    PerPage: {
      type: Number,
      required: true,
    },
  },
  computed: {
    movieId() {
      return this.id + 1 + this.currentPage * this.PerPage - this.PerPage;
    },
    posterBg() {
      return {
        "background-image": `url(${this.movie.Poster})`,
      };
    },
  },
  methods: {
    emitRemoveEvent() {
      this.$emit("removeItem", {
        id: this.movie.imdbID,
        title: this.movie.Title,
      });
    },
    showInfoModalEvent() {
      this.$emit("showModal", this.movie.imdbID);
    },
  },
};
</script>

<style scoped>
.movie-item {
  position: relative;
  cursor: pointer;
  border-radius: 5px;
  overflow: hidden;
  transition: all 0.2s ease;
  height: 500px;
}
.movie-item:hover {
  box-shadow: 0px 5px 30px rgba(0, 0, 0, 0.7);
  transform: scale(1.02);
}
.movie-item-poster {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 40px;
  right: 0;
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  z-index: -1;
}
.movie-bottom-info {
  position: absolute;
  height: 35px;
  left: 0;
  bottom: 0;
  text-overflow: ellipsis;
  z-index: -1;
}
.movie-info-wrap {
  padding: 20px 10px;
  width: 100%;
  opacity: 0;
  transition: all 0.2s ease;
  background-color: rgba(0, 0, 0, 0.9);
}
.movie-title {
  font-size: 20px;
  color: #fff;
}
.movies-rating {
  margin-bottom: 10px;
}
.movie-rating {
  font-size: 12px;
  color: #fff;
}
.movie-item:hover .movie-info-wrap {
  opacity: 1;
}
.movie-buttons {
  width: 100%;
}
.movie-bottom-title {
  font-size: 14px;
  color: #fff;
}

@media (max-width: 1200px) {
  .movie-item {
    height: 400px;
  }
}
@media (max-width: 768px) {
  .movie-item {
    height: 400px;
  }
}
@media (max-width: 600px) {
  .movie-item {
    height: 500px;
  }
}
@media (max-width: 500px) {
  .movie-item {
    height: 450px;
  }
  .movie-bottom-info {
    font-size: 12px;
  }
}
@media (max-width: 450px) {
  .movie-item {
    height: 350px;
  }
}
@media (max-width: 400px) {
  .movie-bottom-info {
    font-size: 10px;
  }
  .movie-item {
    height: 300px;
  }
}
</style>
