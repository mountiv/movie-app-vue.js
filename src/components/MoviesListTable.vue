<template>
  <div class="movies-table" :sticky-header="stickyHeader">
    <BContainer sticky-header>
      <BRow class="text-center movies-table-header">
        <BCol class="text-start title" cols="0" sm="6"> Title </BCol>
        <BCol cols="2"> IMDB </BCol>
        <BCol cols="2"> Tomatoes </BCol>
        <BCol cols="2"> Metacritic </BCol>
      </BRow>
      <div v-for="(movie, key, id) in list" :key="key">
        <BRow class="text-center movie-table-row" v-b-toggle="`${id}`">
          <BCol class="text-start" cols="12" sm="6">
            <BButton class="text-white text-start" variant="transparent"
              >{{ id + movieId + "." }} {{ movie.Title }} ({{
                movie.Year
              }})</BButton
            >
          </BCol>
          <BCol cols="2"> {{ movie.Ratings[0].Value }} </BCol>
          <BCol cols="2">
            {{ movie.Ratings[1] ? movie.Ratings[1].Value : "-" }}
          </BCol>
          <BCol cols="2">
            {{ movie.Ratings[2] ? movie.Ratings[2].Value : "-" }}
          </BCol>
          <BCollapse cols="12" :id="`${id}`" class="mt-2">
            <BCard class="text-light movie-card">
              <BRow no-gutters>
                <BCol md="3">
                  <BCard-img
                    :src="movie.Poster"
                    @mouseover="onMouseOver(movie.Poster)"
                    alt="Image"
                    class="rounded-0"
                  ></BCard-img>
                </BCol>
                <BCol md="9">
                  <BCard-body>
                    <div class="movie-info">
                      <BCard-text>
                        <b class="movie-info-header">Award:</b>
                        {{ movie.Awards }}
                      </BCard-text>
                      <BCard-text>
                        <b class="movie-info-header">Country:</b>
                        {{ movie.Country }}
                      </BCard-text>
                      <BCard-text>
                        <b class="movie-info-header">Genre:</b>
                        {{ movie.Genre }}
                      </BCard-text>
                      <BCard-text>
                        <b class="movie-info-header">Runtime:</b>
                        {{ movie.Runtime }}
                      </BCard-text>
                      <BCard-text>
                        <b class="movie-info-header">Released:</b>
                        {{ movie.Released }}
                      </BCard-text>
                      <BCard-text>
                        <b class="movie-info-header">Director:</b>
                        {{ movie.Director }}
                      </BCard-text>
                      <BCard-text>
                        <b class="movie-info-header">Actors:</b>
                        {{ movie.Actors }}
                      </BCard-text>
                      <BCard-text>
                        <b class="movie-info-header">Writer:</b>
                        {{ movie.Writer }}
                      </BCard-text>
                      <BCard-text>
                        <b class="movie-info-header">Discription:</b>
                        {{ movie.Plot }}
                      </BCard-text>
                    </div>
                  </BCard-body>
                </BCol>
              </BRow>
            </BCard>
          </BCollapse>
        </BRow>
      </div>
    </BContainer>
  </div>
</template>

<script>
export default {
  name: "MoviesList",
  data: () => ({
    stickyHeader: true,
    items: this.list,
  }),
  props: {
    list: {
      type: Object,
      default: () => ({}),
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
  data: () => ({}),
  components: {},
  methods: {
    onMouseOver(poster) {
      console.log(poster);
      this.$emit("changeTablePoster", poster);
    },
  },
  computed: {
    movieId() {
      return 1 + this.currentPage * this.PerPage - this.PerPage;
    },
    movieRating1: function () {
      if (!this.movie.Ratings[1].Value) return "Please Select";

      return this.isUpgrade ? "Upgrade" : "Downgrade";
    },
  },
};
</script>

<style scoped>
.movies-table {
  padding: 40px 20px;
  background-color: rgba(0, 0, 0, 0.9);
  color: #fff;
  border-radius: 10px 10px 0 0;
}
.movie-table-row {
  border-bottom: 1px solid #fff;
}
.movies-table-header {
  margin-bottom: 10px;
}
.movie-card {
  background-color: transparent;
}
.movie-info {
  display: flex;
  flex-direction: column;
  text-align: start;
  font-size: 14px;
}
.card-body {
  padding-top: 0 !important;
}
.movie-info-header {
  font-size: 12px;
}
p {
  margin-bottom: 5px;
}
.btn:focus {
  box-shadow: none !important;
}
@media (max-width: 768px) {
  .movies-table {
    font-size: 12px;
  }
  .movies-table-header {
    font-size: 10px;
    padding: 0;
  }
}
@media (max-width: 576px) {
  .movie-table-row {
    justify-content: center;
  }
  .movies-table-header {
    justify-content: center;
  }
  .title {
    display: none;
  }
  .card-body {
    padding: 0.5rem 0.5rem !important;
  }
}
</style>
