<template>
  <div>
    <!-- Bagian pertama yaitu category -->
    <v-container grid-list-md>
      <v-subheader>
        Random category
        <v-spacer></v-spacer>
        <!-- link ke route categories yang nantinya akan kita definisikan routing dan component nya -->
        <router-link to="/categories"> See All </router-link>
      </v-subheader>
      <v-layout row wrap>
        <!-- variabel categories ini nanti akan kita isi dengan data dari endpoint category -->
        <v-flex v-for="category in categories" xs6 :key="category.id">
          <v-card :to="'/category/' + category.slug">
            <!-- untuk load image supaya lebih rapi akan kita buatkan method getImage -->

            <v-img :src="getImage(category.image)" height="150px">
              <v-container fill-height fluid pa-2>
                <v-layout fill-height>
                  <v-flex xs12 align-end flexbox>
                    <!-- nama category nya akan ditampilkan di sini -->
                    <span
                      class="title white--text text-block"
                      v-text="category.name"
                    ></span>
                  </v-flex>
                </v-layout>
              </v-container>
            </v-img>

            <!-- icon dummy saja, nantinya kamu bisa sesuaikan -->
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn icon>
                <v-icon>mdi-heart</v-icon>
              </v-btn>
              <v-btn icon>
                <v-icon>mdi-gadgetmark</v-icon>
              </v-btn>
              <v-btn icon>
                <v-icon>mdi-share</v-icon>
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>

    <!-- Bagian kedua yaitu Gadget -->
    <v-container grid-list-md>
      <v-subheader>
        Top Gadgets
        <v-spacer></v-spacer>
        <!-- link ke route gadgets yang nantinya akan kita definisikan routing dan component nya -->
        <router-link to="/gadgets"> See All </router-link>
      </v-subheader>
      <v-layout row wrap>
        <!-- data buku kita tampilkan dalam 2 kolom (xs6) -->
        <v-flex v-for="(gadget, index) in gadgets" xs6 :key="index">
          <v-card :to="'/gadget/' + gadget.slug">
            <v-img :src="getImage(gadget.cover)" height="150px">
              <v-container fill-height fluid pa-2>
                <v-layout fill-height>
                  <v-flex xs12 align-end flexbox>
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
                <v-icon> mdi-heart </v-icon>
              </v-btn>
              <v-btn icon>
                <v-icon> mdi-gadgetmark </v-icon>
              </v-btn>
              <v-btn icon>
                <v-icon> mdi-share </v-icon>
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>

<style scoped>
/* mengatur posisi judul */
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
  data: () => ({
    categories: [], //kita definisikan sebagai array kosong
    gadgets: [],
  }),
  methods: {
    getImage(image) {
      if (image != null && image.length > 0) {
        return process.env.VUE_APP_BACKEND_URL + "/images" + image;
      }
      //default image jika tidak ditemukan,
      //image berada pada folder /public/img di project ini
      return "/public/img/notfound.png";
    },
  },
  created() {
    let count = 4;
    //request ke endpoint category random dengan parameter count = 4
    this.axios
      .get("/categories/random/" + count)
      .then((response) => {
        let categories = response.data.data; //data dimasukan kedalam property data categories
        this.categories = categories;
      })
      .catch((error) => {
        let responses = error.response;
        console.log(responses);
      });
    count = 8;
    //request ke endpoint top gadget dengan parameter count = 8
    this.axios
      .get("/gadgets/top/" + count)
      .then((response) => {
        let gadgets = response.data.data;
        this.gadgets = gadgets;
      })
      .catch((error) => {
        let responses = error.response;
        console.log(responses);
      });
  },
};
</script>
