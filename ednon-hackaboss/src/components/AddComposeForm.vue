<template>
  <v-row justify="center">
    <v-dialog v-model="dialog" persistent max-width="900px">
      <v-card>
        <v-card-title>
          <span class="headline">Docker Compose File</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col>
                <v-text-field 
                  v-model="compose.name"
                  label="Name" 
                  required>
                </v-text-field>
                <v-textarea
                  v-model="compose.yaml"
                  solo
                  name="input-7-4"
                  label="Compose">
                </v-textarea>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-alert
          class="ma-2"
          :value="alert"
          color="deep-orange"
          dark
          text
          border="top"
          icon="mdi-fire"
          transition="scale-transition"
        >
          {{alertText}}
        </v-alert>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click.native="close">Close</v-btn>
          <v-btn color="blue darken-1" text @click.native="save">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
  import axios from "axios";
  export default {
    name: "AddComposeForm",
    data: () => ({
      alert: false,
      alertText: '',
      compose: {
        data: null,
        yaml: null,
      }
    }),
    props: {
      dialog: {
        default: false
      },
      config: {
        default: false
      }
    },
    mounted () {
      if (this.config) {
        this.compose = this.config;
        console.log(this.config.id);
      }
    },
    methods: {
      close () {
        this.alert=false;
        this.alertText='';
        this.$emit('update:dialog', false)
      },
      add () {
        axios
        .post(process.env.VUE_APP_BACKEND_URL+'compose/?format=json',
          this.compose)
        .then(() => {
            this.$emit('update:dialog', false);
            this.compose = {
              data: null,
              yaml: null,
            }
            this.alert=false;
          })
        .catch(error => {
          this.alert=true;
          this.alertText= JSON.stringify(error.response.data, undefined, 2);
          console.log(error.response)})
        .finally( () => {
          this.$emit('setLoading', false);
          this.$emit('refresh');
        })
      },
      update (){
        axios
        .put(process.env.VUE_APP_BACKEND_URL + 'compose/' + this.compose.id  + '/',
          this.compose)
        .then(() => {
            this.$emit('update:dialog', false);
            this.alert=false;
          })
        .catch(error => {
          this.alert=true;
          this.alertText= JSON.stringify(error.response.data, undefined, 2);
          console.log(error.response)})
        .finally( () => {
          this.$emit('setLoading', false);
          this.$emit('refresh');
        })
      },
      save () {
        this.$emit('setLoading', true);
        if (this.config) {
          this.update();
        } else {
          this.add();
        }
      }
    },
    watch: {
      
    }
  }
</script>