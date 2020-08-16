<template>
  <div class="home my-4">
    <h1 class="text-center mb-3">
      Malya
    </h1>

    <b-form inline class="justify-content-center" @submit.prevent="onSubmit">
      <label for="inline-form-input-search" class="sr-only">
        Search
      </label>
      <b-input
        class="mr-sm-2"
        placeholder="Search for manga..."
        v-model="search"
      ></b-input>
      <b-button
        variant="outline-secondary"
        type="submit"
        :disabled="search.length < 3"
        >Search</b-button
      >
    </b-form>

    <div class="text-center">
      <Loading v-if="loading"></Loading>
    </div>

    <div class="my-4 container" v-if="searching && results.length">
      <h3>Results for "{{ searchWords }}"</h3>
      <hr />
      <ul class="list-unstyled">
        <b-media
          tag="li"
          v-for="result in results"
          :key="result.slug"
          class="py-3"
        >
          <template v-slot:aside>
            <b-link :to="`/manga/${result.slug}`">
              <b-img
                :src="result.thumbnail"
                width="128"
                alt="placeholder"
              ></b-img>
            </b-link>
          </template>
          <b-link
            :to="`/manga/${result.slug}`"
            class="text-decoration-none text-dark"
          >
            <h5 class="mt-0 mb-1">{{ result.title }}</h5>
            <p class="mb-0 text-muted">{{ result.author }}</p>
            <p class="mb-0 text-muted">Updated : {{ result.updated }}</p>
            <p class="mb-0 text-muted">Views : {{ result.views }}</p>
            <p class="mb-0 text-muted">Rating : {{ result.rating }}</p>
          </b-link>
        </b-media>
      </ul>
    </div>
    <div class="my-4 container" v-else-if="searching && !loading">
      <h3>Results for "{{ searchWords }}"</h3>
      <hr />
      <p>No Results.</p>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Loading from "@/components/Loading.vue";
import axios from "axios";

export default {
  name: "Home",
  components: {
    Loading,
  },
  data() {
    return {
      loading: false,
      searching: false,
      search: "",
      searchWords: "",
      results: [],
    };
  },

  methods: {
    onSubmit() {
      this.loading = true;
      this.searching = true;
      this.resetResults();
      this.fetchResults();
    },

    resetResults() {
      this.results = [];
    },

    fetchResults() {
      this.searchWords = this.search;
      let query = this.search.replace(/ /g, "_");

      axios
        .get(
          `https://sleepy-retreat-88143.herokuapp.com/api/manga/search/${query}`
        )
        .then((response) => {
          this.results = response.data;
        })
        .catch()
        .finally(() => {
          this.loading = false;
        });
    },
  },
};
</script>
