<template>
    <v-footer dark height="auto" color="secondary">
        <v-flex xs12>
            <v-card flat tile class="white--text text-center" color="secondary">
                <!-- menu icon sosial media -->
                <v-card-text>
                    <v-btn v-for="icon in icons" :key="icon" class="mx-4" icon>
                        <v-icon size="24px">{{ icon }}</v-icon>
                    </v-btn>
                </v-card-text>

                <v-layout justify-center row wrap>
                    <!-- link menu halaman aplikasi -->
                    <v-btn v-for="link in links" :key="link" color="white" text rounded>
                        {{link}}
                    </v-btn>

                    <!-- text copyright -->
                    <v-flex py-3 white--text xs12 color="primary">
                        &copy;2020 - Gadget Store powered by <strong>Vuetify</strong>
                    </v-flex>
                </v-layout>
            </v-card>
        </v-flex>
    </v-footer>
</template>
<script>
    export default {
        data: () => ({
            //variable icon sosial media yang ingin ditampilkan
            icons: [
                'fab fa-facegadget',
                'fab fa-twitter',
                'fab fa-google-plus',
                'fab fa-instagram'
            ],
            //variable link halaman yang ingin ditampilkan
            links: [
                'Home',
                'About Us',
                'Team',
                'Services',
                'Blog',
                'Contact Us'
            ]
        }),
    }
</script>