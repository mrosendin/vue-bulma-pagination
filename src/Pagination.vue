<template>
  <div id="pagination">
    <nav class="pagination is-centered">
      <a class="pagination-previous">Previous</a>
      <a class="pagination-next">Next page</a>
      <ul class="pagination-list">
        <li><a class="pagination-link">1</a></li>
        <li><span class="pagination-ellipsis">&hellip;</span></li>
        <li><a class="pagination-link">45</a></li>
        <li><a class="pagination-link is-current">46</a></li>
        <li><a class="pagination-link">47</a></li>
        <li><span class="pagination-ellipsis">&hellip;</span></li>
        <li><a class="pagination-link">86</a></li>
      </ul>
    </nav>
  </div>
</template>

<script>
export default {
  name: 'pagination',
  props: {
    current: {
      type: Number
    },
    total: {
      type: Number,
      default: 0
    },
    itemsPerPage: {
      type: Number
    },
    onChange: {
      type: Function
    }
  },
  data () {
    return {}
  }
}
</script>

<style lang="sass" scoped>
@import "../node_modules/bulma/sass/utilities/_all"
@import "../node_modules/bulma/sass/components/pagination"
</style>
