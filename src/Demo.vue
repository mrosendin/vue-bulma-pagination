<template>
  <div id="demo">
    <section class="hero is-primary">
      <div class="hero-body">
        <div class="container">
          <h1 class="title">
            Vue Bulma Pagination Demo
          </h1>
        </div>
      </div>
    </section>

    <div class="container">

      <div class="notification is-danger" v-if="error">
        <button class="delete" @click="error = ''"></button>
        {{ error }}
      </div>

      <table class="table">
        <thead>
          <tr>
            <th v-for="(key, value) in countries[0]">{{ key }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="country in countries">
            <td v-for="key in keys">{{ country[key] }}</td>
          </tr>
        </tbody>
      </table>

      <!-- Start Pagination Component -->
      <pagination
        :current="config.current"
        :total="config.total">
      </pagination>
      <!-- End Pagination Component -->

    </div>
  </div>
</template>

<script>
import Pagination from './Pagination.vue'
import axios from 'axios';

const config = {
  current: 1,         // Current page
  total: null,        // Items total count
  itemsPerPage: 10,   // Items per page
  onChange: () => {   // Page change callback
    console.log('Calling onChange')
  }
}

export default {
  name: 'demo',
  components: { Pagination },
  data () {
    return {
      countries: [],
      config: config,
      error: ''
    }
  },
  created () {
    axios.get('https://api.openaq.org/v1/countries?limit=10')
    .then(response => {
      console.log(response.data)
      this.keys = Object.keys(response.data.results[0])
      this.countries = response.data.results

      // Set pagination config based on response
      this.config.total = response.data.meta.found
    })
    .catch(error => {
      this.error = error
    })
  }
}
</script>
