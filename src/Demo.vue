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
        :current="pagination.current"
        :total="pagination.total"
        :itemsPerPage="pagination.itemsPerPage"
        :onChange="onChange">
      </pagination>
      <!-- End Pagination Component -->

    </div>
  </div>
</template>

<script>
import Pagination from './Pagination.vue'
import axios from 'axios';

const pagination = {
  current: 1,      // Current page
  total: 0,     // Items total count
  itemsPerPage: 5  // Items per page
}

export default {
  name: 'demo',
  components: { Pagination },
  data () {
    return {
      countries: [],
      pagination: pagination,
      error: ''
    }
  },
  methods: {
    onChange (page) {
      console.log(`Getting page ${page}`)
      axios.get(`https://api.openaq.org/v1/countries?limit=5&page=${page}`)
      .then(response => {
        this.countries = response.data.results
        this.pagination.current  = page
      })
      .catch(error => {
        this.error = error
      })
    }
  },
  created () {
    axios.get('https://api.openaq.org/v1/countries?limit=5')
    .then(response => {
      this.keys = Object.keys(response.data.results[0])
      this.countries = response.data.results

      // Set pagination config based on response
      this.pagination.total = response.data.meta.found
    })
    .catch(error => {
      this.error = error
    })
  }
}
</script>

<style scoped>
.hero {
  margin-bottom: 35px;
}
</style>
