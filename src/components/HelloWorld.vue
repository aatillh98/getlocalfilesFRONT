<!-- DirectoryListing.vue -->
<template>
  <div>
    <b-container fluid="md">
      <b-row>
        <b-col cols="8">
          <b-form-input v-model="directory" placeholder="Enter your name"></b-form-input>
        </b-col>
        <b-col>
          <b-button @click="getDirectoryListing" variant="success" :disabled="disabledButton">Get Listing</b-button>
        </b-col>
        <b-col></b-col>
      </b-row>
      <b-row class="mt-4">
        <b-col></b-col>
        <b-col>
          <b-list-group v-for="item in listing" :key="item">
        <b-list-group-item>{{ item }}</b-list-group-item>
      </b-list-group>
        </b-col>
        <b-col></b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import axios from 'axios'
import { BFormInput, BListGroup, BButton, BCol, BRow, BContainer, BListGroupItem } from 'bootstrap-vue'
export default {
  components: {
    BFormInput,BListGroup,BButton, BCol, BRow, BContainer, BListGroupItem
  },
  data() {
    return {
      directory: '',
      listing: [],
      disabledButton: false
    };
  },
  watch: {
    directory(val) {
      if (val == '') {
        this.disabledButton = true
        this.listing = []
      } else {
        this.disabledButton = false
      }
    }
  },
  methods: {
    getDirectoryListing() {
      axios.get('http://getlocalfiles.local/api/get-directory-listing', { params: { directory: this.directory } })
        .then(response => {
          this.listing = response.data.listing;
          this.disabledButton = true
        })
        .catch(error => {
          console.error('Error fetching directory listing', error);
        });
    },
    enabledButton() {
      this.disabledButton = false;
    }
  },
};
</script>
