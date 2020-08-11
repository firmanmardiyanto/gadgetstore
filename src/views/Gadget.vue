<template>
  <div class="about">
    <v-container>
      <v-subheader class="title"> {{ gadget.merk }} </v-subheader>
      <v-img
        v-if="gadget.cover"
        :src="getImage(gadget.cover)"
        height="200px"
        contain
      ></v-img>
      <v-subheader> Information </v-subheader>
      <table class="v-data-table">
        <tbody>
          <tr>
            <th class="text-xs-left">Seller</th>
            <td>{{ gadget.seller }}</td>
          </tr>
          <tr>
            <th class="text-xs-left">Price</th>
            <td v-if="gadget.price">
              Rp. {{ gadget.price.toLocaleString("id-ID") }}
            </td>
          </tr>
          <tr>
            <th class="text-xs-left">Weight</th>
            <td>{{ gadget.weight }} kg</td>
          </tr>
          <tr>
            <th class="text-xs-left">Stock</th>
            <td>{{ gadget.stock }}</td>
          </tr>
          <tr>
            <th class="text-xs-left">Categories</th>
            <td>
              <template
                v-for="category in gadget.categories"
                v-key="category.id"
              >
                {{ category.name }},
              </template>
            </td>
          </tr>
        </tbody>
      </table>
      <v-subheader> Description </v-subheader>
      <div class="body-2" v-html="gadget.description"></div>
      <div style="position:relative">
        <v-btn block color="success" @click="buy" :disabled="gadget.stock == 0">
          <v-icon> save_alt </v-icon> &nbsp; buy
        </v-btn>
      </div>
    </v-container>
  </div>
</template>

<script>
import { mapActions } from "vuex";
export default {
  data() {
    return {
      gadget: {},
    };
  },
  methods: {
    ...mapActions({
      addCart: "cart/add",
      setAlert: "alert/set",
    }),
    buy() {
      this.addCart(this.gadget);
      this.setAlert({
        status: true,
        text: "Added to cart",
        type: "success",
      });
    },
  },
  created() {
    let slug = this.$route.params.slug;
    this.axios
      .get("/gadgets/slug/" + slug)
      .then((response) => {
        let gadget = response.data.data;
        this.gadget = gadget;
      })
      .catch((error) => {
        let responses = error.response;
        console.log(responses);
      });
  },
};
</script>
