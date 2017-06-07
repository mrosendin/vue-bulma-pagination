<template>
  <li><span class="pagination-ellipsis">&hellip;</span></li>
</template>

<script>
export default {
  name: 'ellipse-break',
  props: ['page', 'current', 'onChange']
}
</script>

<style lang="sass" scoped>
@import "../node_modules/bulma/sass/utilities/_all"
@import "../node_modules/bulma/sass/components/pagination"
</style>
