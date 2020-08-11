<template>
  <div>
    <v-container grid-list-md>
      <v-subheader> {{ category.name }} phones</v-subheader>
      <v-img
        v-if="category.image"
        :src="getImage(category.image)"
        height="200px"
      ></v-img>
      <v-subheader> Gadget by "{{ category.name }}" category"</v-subheader>
      <v-layout row wrap>
        <!--data yang berelasi akan ditampilkan menggunakan looping -->
        <v-flex v-for="gadget in gadgets" xs6 md3 :key="gadget.id">
          <v-card :to="'/gadget/' + gadget.slug">
            <v-img
              v-if="gadget.cover"
              :src="getImage(gadget.cover)"
              height="175px"
              contain
            >
              <v-container fill-height fluid pa-2>
                <v-layout fill-height>
                  <v-flex sx12 align-end flexbox>
                    <span
                      class="title white--text text-block"
                      v-text="gadget.merk"
                    ></span>
                  </v-flex>
                </v-layout>
              </v-container>
            </v-img>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn icon>
                <v-icon>favorite</v-icon>
              </v-btn>
              <v-btn icon>
                <v-icon>bookmark</v-icon>
              </v-btn>
              <v-btn icon>
                <v-icon>share</v-icon>
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-flex>
      </v-layout>
      <template>
        <div class="text-xs-center">
          <v-pagination
            v-model="page"
            @input="go"
            :length="lengthPage"
            :total-visible="5"
          ></v-pagination>
        </div>
      </template>
    </v-container>
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
      category: {},
      gadgets: [],
      page: 0,
      lengthPage: 0,
    };
  },
  methods: {
    go() {
      let slug = this.$route.params.slug;
      let url = "categories/slug/" + slug;
      if (this.page > 0) url = url + "?page=" + this.page;
      url = encodeURI(url);
      this.axios
        .get(url)
        .then((response) => {
          let response_data = response.data;
          let category = response_data.data;
          this.category = category;
          this.gadgets = category.gadgets.data;
          this.lengthPage = category.gadgets.last_page;
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
