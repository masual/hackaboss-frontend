<template>
  <v-card
    class="mx-auto"
    outlined
  >
    <AddComposeForm :dialog.sync="dialog" v-on:refresh="refresh" v-on:setLoading="setLoading" :config="config" ></AddComposeForm>
    <v-list-item>
      <v-list-item-content>
        <v-list-item-title class="headline mb-1">{{config.name}}</v-list-item-title>
      </v-list-item-content>
    </v-list-item>
    <v-expansion-panels>
        <v-expansion-panel>
        <v-expansion-panel-header>YAML</v-expansion-panel-header>
        <v-expansion-panel-content>
            <v-textarea
                solo
                :disabled="true"
                :value=config.yaml
                label="Compose"
                ></v-textarea>
        </v-expansion-panel-content>
        </v-expansion-panel>
    </v-expansion-panels>
    <v-card-actions>
      <v-btn text color="primary" @click="dialog=true">Edit</v-btn>
      <v-btn text color="error" @click="deleteCard">Delete</v-btn>
    </v-card-actions>
  </v-card>
</template>


<script>
  import axios from "axios";
  import AddComposeForm from './AddComposeForm';
  
  export default {
    name: "ComposeCard",
    props: {
        config: {
        default: null
      }
    },
    data: () => ({
        dialog: false
    }),
    methods: {
      deleteCard () {
        axios
        .delete(process.env.VUE_APP_BACKEND_URL + 'compose/' + this.config.id,
          this.compose)
        .then(response => console.log(response))
        .catch(error => console.log(error))
        .finally( () => {
          this.$emit('refresh');
        })
      },
      refresh (){
        this.$emit('refresh');
      },
      setLoading (value) {
        this.$emit('setLoading', value);
      }
    },
    components: {
      AddComposeForm
    }
  }
</script>