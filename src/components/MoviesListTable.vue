<template>
  <div class="movies-table">
    <BContainer>
      <BRow class="text-center movies-table-header">
        <BCol class="text-start" cols="6"> Title </BCol>
        <BCol cols="2"> IMDB </BCol>
        <BCol cols="2"> Tomatoes </BCol>
        <BCol cols="2"> "Metacritic" </BCol>
      </BRow>
      <div v-for="(movie, key, id) in list" :key="key">
        <BRow class="text-center">
          <BCol class="text-start" cols="6">
            {{ id + movieId + "." }} {{ movie.Title }} ({{ movie.Year }})
          </BCol>
          <BCol cols="2"> {{ movie.Ratings[0].Value }} </BCol>
          <BCol cols="2"> {{ movie.Ratings[1].Value }} </BCol>
          <BCol cols="2"> {{ movie.Ratings[2].Value }} </BCol>
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
  },
};
</script>

<style scoped>
.movies-table {
  padding: 40px 20px;
  background-color: rgba(0, 0, 0, 0.9);
  color: #fff;
}
.movies-table-header {
  margin-bottom: 10px;
}
</style>
