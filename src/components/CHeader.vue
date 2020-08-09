<template>
  <!-- toolbar vutify dengan warna primary -->
  <v-app-bar>
    <!-- header bagian kiri -->
    <v-app-bar-nav-icon
      v-if="isHome"
      @click="setSideBar(!sideBar)"
    ></v-app-bar-nav-icon>
    <v-btn v-if="!isHome" icon @click="$router.go(-1)">
      <v-icon>mdi-keyboard-backspace</v-icon>
    </v-btn>

    <!-- header bagian tengah -->
    <v-toolbar-title class="white-text">Gadget Store</v-toolbar-title>

    <!-- separator supaya header bagian kanan rata kanan -->
    <v-spacer></v-spacer>
    <v-text-field
      v-if="isHome"
      @click="search()"
      dense
      hide-details
      append-icon="mic"
      text
      label="Search"
      prepend-inner-icon="search"
      solo-inverted
    ></v-text-field>
    <!-- header bagian kanan -->
    <v-btn icon @click="cart()">
      <v-badge left overlap color="green">
        <span slot="badge" v-if="countCart > 0">{{ countCart }}</span>
        <span slot="badge" v-else>0</span>
        <v-icon>mdi-shopping-outline</v-icon>
      </v-badge>
    </v-btn>
  </v-app-bar>
</template>
<script>
import { mapGetters, mapActions } from "vuex";
export default {
  name: "c-header",
  methods: {
    ...mapActions({
      setSideBar: "setSideBar",
      setStatusDialog: "dialog/setStatus",
      setComponent: "dialog/setComponent",
    }),
    search() {
      this.setStatusDialog(true);
      this.setComponent("search");
      this.setSideBar(false);
    },
    cart() {
      this.setStatusDialog(true);
      this.setComponent("cart");
      this.setSideBar(false);
    },
  },
  computed: {
    ...mapGetters({
      sideBar: "sideBar",
      countCart: "cart/count",
    }),
    isHome() {
      return this.$route.path === "/";
    },
  },
};
</script>
