<template>
  <div class="manga my-4 container-fluid">
    <div class="row">
      <div class="col-lg-5">
        <b-card class="mb-3 shadow-sm">
          <div v-if="loading" class="text-center">
            <Loading v-if="loading"></Loading>
          </div>
          <div v-else>
            <b-media>
              <template v-slot:aside>
                <b-img :src="details.thumbnail" width="128" alt="placeholder" rounded></b-img>
              </template>
              <h4>{{ details.title }}</h4>
              <p class="text-muted">{{ details.alternatives }}</p>

              <b-button block :to="`${slug}/chapter/${last.endpoint}`">Read the last chapter</b-button>
              <b-button
                block
                variant="warning"
                :to="`${slug}/chapter/${first.endpoint}`"
                class="mb-2"
              >Read the first chapter</b-button>
            </b-media>
            <hr />
            <dl class="row">
              <dt class="col-md-4 text-muted">Updated :</dt>
              <dd class="col-md-8">{{ details.updated }}</dd>
              <dt class="col-md-4 text-muted">Status :</dt>
              <dd class="col-md-8">{{ details.status }}</dd>
              <dt class="col-md-4 text-muted">Author(s) :</dt>
              <dd class="col-md-8">{{ details.author }}</dd>
              <dt class="col-md-4 text-muted">View(s) :</dt>
              <dd class="col-md-8">{{ details.views }}</dd>
              <dt class="col-md-4 text-muted">Genre(s) :</dt>
              <dd class="col-md-8">
                <b-badge
                  pill
                  variant="warning"
                  class="mr-2"
                  v-for="genre in details.genres"
                  :key="genre.slug"
                >{{ genre.genre }}</b-badge>
              </dd>
            </dl>
          </div>
        </b-card>

        <b-card class="mb-3 shadow-sm">
          <div v-if="loading" class="text-center">
            <Loading v-if="loading"></Loading>
          </div>
          <div v-else>
            <h3>Description</h3>
            <hr />
            {{ details.description }}
          </div>
        </b-card>
      </div>

      <div class="col-lg-7">
        <b-card class="mb-3 shadow-sm">
          <div class="overflow-auto" style="max-height:638px">
            <table class="table table-hover">
              <thead>
                <tr>
                  <th>Chapter Name</th>
                  <th>Uploaded</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td v-if="loading" colspan="2" class="text-center">
                    <Loading></Loading>
                  </td>
                </tr>
                <tr v-for="chapter in details.chapters" :key="chapter.endpoint">
                  <th scope="row">
                    <a
                      :href="`${slug}/chapter/${chapter.endpoint}`"
                      class="text-dark"
                    >{{ chapter.title }}</a>
                  </th>
                  <td>{{ chapter.uploaded }}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </b-card>
      </div>
    </div>
  </div>
</template>

<script>
import Loading from "@/components/Loading.vue";
import axios from "axios";
export default {
  name: "Manga",
  props: ["slug"],
  components: {
    Loading,
  },
  created() {
    this.getDetails();
  },

  data() {
    return {
      details: [],
      chapters: [],
      loading: false,
      first: "",
      last: "",
    };
  },

  methods: {
    getDetails() {
      this.loading = true;
      axios
        .get(
          `https://sleepy-retreat-88143.herokuapp.com/api/manga/details/${this.slug}`
        )
        .then((response) => {
          this.details = response.data;
          this.chapters = this.details.chapters;
          this.first = this.chapters[this.chapters.length - 1];
          this.last = this.chapters[0];
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
