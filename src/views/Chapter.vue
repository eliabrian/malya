<template>
  <div class="chapter container my-4">
    <h3>{{ result.title }}</h3>
    <div v-for="image in result.images" :key="image.image">
      <b-img-lazy v-bind="mainProps" :src="image.image" :alt="image.title"></b-img-lazy>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Chapter",
  props: ["slug", "chapter"],
  created() {
    this.getImages();
  },

  data() {
    return {
      result: [],
      loading: false,
      mainProps: {
        center: true,
        fluidGrow: true,
        blank: true,
        blankColor: "#bbb",
        class: "my-5",
      },
    };
  },

  methods: {
    getImages() {
      this.loading = true;
      axios
        .get(
          `https://sleepy-retreat-88143.herokuapp.com/api/chapter/${this.slug}/${this.chapter}`
        )
        .then((response) => {
          this.result = response.data;
        })
        .catch((error) => {
          console.log(error);
        })
        .finally(() => {
          this.loading = false;
        });
    },
  },
};
</script>