<template>
  <v-app id="inspire">
    <v-navigation-drawer
      v-model="drawer"
      app
      clipped
    >
      <v-list dense>
        <v-list-item link @click="dialog=true">
          <v-list-item-action>
            <v-icon>mdi-playlist-plus</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Add Compose</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <AddComposeForm :dialog.sync="dialog" v-on:refresh="refresh" v-on:setLoading="setLoading"></AddComposeForm>
      </v-list>
      <v-spacer></v-spacer>
      <v-container fluid>
        <v-col align="center"
          justify="center">
          <v-row class="mt-5" align="center"
          justify="center">
            <a href="https://hackaboss.com/">
              <v-img src="@/assets/hackaboss.png" width="100%"></v-img>
            </a>
           </v-row>
          <v-row align="center"
          justify="center">
            <a href="https://ednon.com/ednon-labs/">
              <v-img src="@/assets/labs.png" width="75%"></v-img>
            </a>
          </v-row>
        </v-col>
      </v-container>
    </v-navigation-drawer>

    <v-app-bar
      app
      clipped-left
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>Docker Compose Manager</v-toolbar-title>
    </v-app-bar>

    <v-content>
      <v-container
        fluid
      >
      <v-dialog
      v-model="loading"
      hide-overlay
      persistent
      width="300"
        >
          <v-card
            color="primary"
            dark
          >
            <v-card-text>
              Loading
              <v-progress-linear
                indeterminate
                color="black"
                class="mb-0"
              ></v-progress-linear>
            </v-card-text>
          </v-card>
        </v-dialog>
        <v-row dense>
          <v-col
            v-for="compose in compose_list"
            :key="compose.id"
            :cols="6"
          >
          <ComposeCard 
            v-on:refresh="refresh()"
            :config="compose" 
          ></ComposeCard>

          </v-col>
        </v-row>
      </v-container>
    </v-content>

    <v-footer app>
      <span>&copy; 2020 Ednon | v - {{ version }}</span>
    </v-footer>
  </v-app>
</template>

<script>
  import AddComposeForm from './components/AddComposeForm';
  import ComposeCard from './components/ComposeCard';
  import axios from "axios";

  export default {
    props: {
      source: String,
    },

    data: () => ({
      version: process.env.VUE_APP_VERSION,
      loading: false,
      drawer: null,
      dialog: false,
      compose_list: []
    }),

    created () {
      this.$vuetify.theme.dark = true
    },
    methods: {
      refresh () {
        axios
          .get(process.env.VUE_APP_BACKEND_URL+'compose/?format=json')
          .then(response => (this.compose_list = response.data))
          .catch(error => console.log(error))
      },
      setLoading (value) {
        this.loading=value;
      }
    },
    mounted () {
      this.refresh();
    },
    components: {
      AddComposeForm,
      ComposeCard
    }
  }
</script>