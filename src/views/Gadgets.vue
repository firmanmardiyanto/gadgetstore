<template>
    <div>
        <v-container grid-list-md>
            <v-subheader>
                All Gadgets
            </v-subheader>
            <v-layout row wrap>
                <v-flex v-for="(gadget, index) in gadgets" xs6 :key="index">
                    <v-card :to="'/gadget/'+ gadget.slug">
                        <v-img :src="getImage(gadget.cover)" height="150px">
                            <v-container fill-height fluid pa-2>
                                <v-layout fill-height>
                                    <v-flex xs12 align-end flexbox>
                                        <span class="title white--text text-block" v-text="gadget.merk"></span>
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

        <template>
            <div class="text-xs-center">
                <v-pagination v-model="page" @input="go" :length="lengthPage" :total-visible="4" circle></v-pagination>
            </div>
        </template>
    </div>
</template>

<style scoped>
    .text-block {
        position: absolute;
        bottom: 5px;
        Left: 5px;
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
                gadgets: [],
                page: 0,
                lengthPage: 0
            }
        },
        methods: {
            go() {
                let url = '/gadgets'
                if (this.page > 0) url = '/gadgets?page=' + this.page
                this.axios.get(url)
                    .then((response) => {
                        let response_data = response.data
                        let gadgets = response_data.data
                        this.lengthPage = response_data.meta
                            .last_page // jumlah halaman di dapat dari meta endpoint gadgets
                        this.gadgets = gadgets // daftar gadgets dari endpoint categories
                    })
                    .catch((error) => {
                        console.log(error.response)
                    })
            },
        },
        created() {
            this.go()
        }
    }
</script>