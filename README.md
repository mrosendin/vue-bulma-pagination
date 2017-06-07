# Vue Bulma Pagination

> A Vue.js pagination component for the Bulma CSS framework

## Installation

Install via NPM:

``` bash
npm install vue-bulma-pagination --save
```

## Usage

See the [example](https://github.com/roseware/vue-bulma-pagination/blob/master/src/Demo.vue).

``` html
<template>

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

  <pagination
    :current="current"
    :total="total"
    :itemsPerPage="itemsPerPage"
    :onChange="onChange">
  </pagination>

</template>

<script>
import Pagination from './Pagination.vue'
import axios from 'axios';

const pagination = {
  current: 1,       // Current page
  total: 0,         // Items total count
  itemsPerPage: 5   // Items per page
}

export default {
  name: 'demo',
  components: { Pagination },
  data () {
    return {
      countries: [],
      pagination: pagination
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
  }
}
</script>
```

## Development

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev
```

For detailed explanation on how things work, consult the [docs for vue-loader](http://vuejs.github.io/vue-loader).
