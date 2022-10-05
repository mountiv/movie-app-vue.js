<template>
  <div class="movies-table">
    <BContainer>
      <BRow class="text-center movies-table-header">
        <BCol class="text-start" cols="6"> Title </BCol>
        <BCol cols="2"> IMDB </BCol>
        <BCol cols="2"> Tomatoes </BCol>
        <BCol cols="2"> Metacritic </BCol>
      </BRow>
      <div v-for="(movie, key, id) in list" :key="key">
        <BRow class="text-center movie-table-row">
          <BCol class="text-start" cols="6">
            <BButton v-b-toggle="`${id}`" variant="primary"
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
          <b-collapse cols="12" id="`${id}`" class="mt-2">
            <b-card class="text-danger">
              <p class="card-text">Collapse contents Here</p>
            </b-card>
          </b-collapse>
        </BRow>
      </div>
    </BContainer>
  </div>
</template>

<script>
export default {
  name: "MoviesList",
  data: () => ({
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
  methods: {},
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
@media (max-width: 768px) {
  .movies-table {
    font-size: 12px;
  }
  .movies-table-header {
    font-size: 10px;
    padding: 0;
  }
}
</style>
