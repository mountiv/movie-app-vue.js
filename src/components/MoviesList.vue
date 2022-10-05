<template>
  <div>
    <BContainer>
      <h3 class="list-title">{{ listTitle }}</h3>
      <BTabs active-nav-item-class="text-dark" content-class="mt-3 mb-3" lazy>
        <BTab
          title="Covers"
          :title-link-class="'text-light'"
          active
          @click="onCoversClick"
        >
          <BRow>
            <template v-if="isExist">
              <BCol
                cols="6"
                md="4"
                lg="3"
                v-for="(movie, key, id) in list"
                :key="key"
              >
                <MovieItem
                  :movie="movie"
                  :id="id"
                  :currentPage="currentPage"
                  :PerPage="moviesPerPage"
                  @mouseover.native="onMouseOver(movie.Poster)"
                  @removeItem="onRemoveItem"
                  @showModal="onShowMovieInfo"
                />
              </BCol>
            </template>
            <template v-else>
              <div>Empty List</div>
            </template>
          </BRow>
          <BModal
            body-class="movie-modal-body"
            :id="movieInfoModalID"
            size="xl"
            hide-footer
            hide-header
          >
            <ModalInfoContent
              :movie="selectedMovie"
              @closeModal="onCloseModal"
            /> </BModal
        ></BTab>
        <BTab
          :title-link-class="'text-light'"
          title="Table"
          @click="onTableClick"
        >
          <MoviesListTable
            :list="list"
            :currentPage="currentPage"
            :PerPage="moviesPerPage"
          />
        </BTab>
      </BTabs>
    </BContainer>
  </div>
</template>

<script>
import MoviesListTable from "./MoviesListTable.vue";
import MovieItem from "./MovieItem.vue";
import { mapActions, mapGetters } from "vuex";
import ModalInfoContent from "@/components/MovieInfoModalContent.vue";

export default {
  name: "MoviesList",
  props: {
    list: {
      type: Object,
      default: () => ({}),
    },
  },
  data: () => ({
    movieInfoModalID: "movie-info",
    selectedMovieID: "",
  }),
  components: {
    MoviesListTable,
    MovieItem,
    ModalInfoContent,
  },
  methods: {
    ...mapActions("movies", [
      "removeMovie",
      "changeMoviesPerPage",
      "changeCurrentPage",
    ]),
    ...mapActions(["showNotify"]),
    onMouseOver(poster) {
      this.$emit("changePoster", poster);
    },
    async onRemoveItem({ id, title }) {
      const isConfirmed = await this.$bvModal.msgBoxConfirm(
        `Do you want to delete ${title}?`
      );

      if (isConfirmed) {
        this.removeMovie(id);
        this.showNotify({
          msg: "Movie deleted successful",
          variant: "success",
          title: "Success",
        });
      }
    },
    onShowMovieInfo(id) {
      this.selectedMovieID = id;
      this.$bvModal.show(this.movieInfoModalID);
    },
    onCloseModal() {
      this.selectedMovieID = null;
      this.$bvModal.hide(this.movieInfoModalID);
    },
    onCoversClick() {
      this.changeCurrentPage(1);
      this.changeMoviesPerPage(12);
    },
    onTableClick() {
      this.changeCurrentPage(1);
      this.changeMoviesPerPage(50);
    },
  },
  computed: {
    ...mapGetters("movies", ["isSearch", "currentPage", "moviesPerPage"]),
    isExist() {
      return Boolean(Object.keys(this.list).length);
    },
    listTitle() {
      return this.isSearch ? "Search result" : "IMDB Top 250 Greatest movies";
    },
    selectedMovie() {
      return this.selectedMovieID ? this.list[this.selectedMovieID] : null;
    },
  },
};
</script>

<style scoped>
.list-title {
  font-size: 50px;
  margin-bottom: 30px;
  color: #fff;
}
.movie-modal-body {
  padding: 0 !important;
}

@media (max-width: 1000px) {
  .list-title {
    font-size: 40px;
  }
}
@media (max-width: 800px) {
  .list-title {
    font-size: 30px;
  }
}
@media (max-width: 600px) {
  .list-title {
    font-size: 20px;
  }
}
</style>
