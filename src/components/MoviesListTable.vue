<template>
  <div>
    <BContainer> Info </BContainer>
  </div>
</template>

<script>
import { mapActions, mapGetters } from "vuex";

export default {
  name: "MoviesList",
  props: {},
  data: () => ({}),
  components: {},
  methods: {
    ...mapActions("movies", ["removeMovie"]),
  },
  computed: {
    ...mapGetters("movies", ["isSearch", "currentPage", "moviesPerPage"]),
  },
};
</script>

<style scoped></style>
