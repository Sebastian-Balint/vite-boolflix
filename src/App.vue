<script>
import AppSearchbar from "./components/AppSearchbar.vue";
import AppCard from "./components/AppCard.vue";
import { store } from "./store.js";
import axios from "axios";
export default {
  components: {
    AppSearchbar,
    AppCard,
  },
  data() {
    return {
      store,
    };
  },
  methods: {
    getApi() {
      store.film = [];
      axios
        .get(
          `https://api.themoviedb.org/3/search/movie?api_key=a9f93794d83843cad47d4bcbc4f86888&query=${store.inputValue}`
        )
        .then((res) => {
          for (let i = 0; i < res.data.results.length; i++) {
            const data = res.data.results[i];
            const name = data.title;
            const type = "Film";
            const originalName = data.original_title;
            const lang = data.original_language;
            const rating = data.vote_average;
            const imgPath = data.backdrop_path;
            const stars = Math.ceil(rating / 2);
            const emptyStars = 5 - stars;
            store.film.push({
              name,
              type,
              originalName,
              lang,
              rating,
              imgPath,
              stars,
              emptyStars,
            });
          }
        });

      axios
        .get(
          `https://api.themoviedb.org/3/search/tv?api_key=a9f93794d83843cad47d4bcbc4f86888&query=${store.inputValue}`
        )
        .then((res) => {
          for (let i = 0; i < res.data.results.length; i++) {
            const data = res.data.results[i];
            const name = data.title;
            const type = "Serie Tv";
            const originalName = data.original_title;
            const lang = data.original_language;
            const rating = data.vote_average;
            const imgPath = data.poster_path;
            const stars = Math.ceil(rating / 2);
            const emptyStars = Math.floor(5 - rating / 2);
            store.film.push({
              name,
              type,
              originalName,
              lang,
              rating,
              imgPath,
              stars,
              emptyStars,
            });
          }
        });
    },
  },
};
</script>
<template>
  <AppSearchbar @search="getApi" />
  <AppCard />
</template>
<style></style>