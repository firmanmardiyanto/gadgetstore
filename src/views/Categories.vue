<template>
  <div>
    <v-container grid-list-md>
      <v-subheader>
        All Category
      </v-subheader>
      <v-layout row wrap>
        <!-- lakukan perulangan pada properti categories -->
        <v-flex v-for="category in categories" xs12 md3 :key="category.id">
          <v-card :to="'/category/' + category.slug">
            <v-img
              v-if="category.image"
              :src="getImage(category.image)"
              height="150px" contain
            >
              <v-container fill-height fluid pa-2>
                <v-layout fill-height>
                  <v-flex xs12 align-end flexbox>
                    <span
                      class="title white--text text-block"
                      v-text="category.name"
                    ></span>
                  </v-flex>
                </v-layout>
              </v-container>
            </v-img>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn icon>
                <v-icon> mdi-heart </v-icon>
              </v-btn>
              <v-btn icon>
                <v-icon> mdi-bookmark </v-icon>
              </v-btn>
              <v-btn icon>
                <v-icon> mdi-share </v-icon>
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
    <div class="text-xs-center">
      <!-- kode untuk link paging halaman -->
      <v-pagination
        v-model="page"
        @input="go"
        :length="lengthPage"
        :total-visible="4"
        circle
      ></v-pagination>
    </div>
  </div>
</template>

<style scoped>
.text-block {
  position: absolute;
  bottom: 5px;
  left: 5px;
  background-color: black;
  padding-left: 5px;
  padding-right: 5px;
  opacity: 0.7;
  width: 100%;
}
</style>
<script>
export default {
  data() {
    return {
      categories: [],
      page: 0,
      lengthPage: 0,
    };
  },
  methods: {
    go() {
      let url = "/categories";
      if (this.page > 0) url = "/categories?page=" + this.page;
      this.axios
        .get(url)
        .then((response) => {
          let response_data = response.data;
          let categories = response.data.data;
          this.lengthPage = response_data.meta.last_page; //Jumlah halaman di dapat dari meta endpoint categories
          this.categories = categories; //daftar categories dari endpoint categories
        })
        .catch((error) => {
          console.log(error.response);
        });
    },
  },
  created() {
    this.go();
  },
};
</script>
