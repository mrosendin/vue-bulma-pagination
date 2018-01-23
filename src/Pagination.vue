<template>
  <div id="pagination">
    <nav class="pagination is-centered">
      <a class="pagination-previous" v-if="current > 1" @click="onChange(current - 1)">&larr; Previous</a>
      <a class="pagination-next" v-if="size > 1 && current < size" @click="onChange(current + 1)">Next &rarr;</a>
      <ul class="pagination-list">
        <component v-for="element in elements" :key="element"  :is="element.type" :page="element.page" :current="current" :onChange="onChange"/>
      </ul>
    </nav>
  </div>
</template>

<script>
import Page from './Page.vue'
import EllipseBreak from './EllipseBreak.vue'

export default {
  name: 'pagination',
  components: { Page, EllipseBreak },
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
    step: {
      type: Number,
      default: 3
    },
    onChange: {
      type: Function
    }
  },
  data () {
    return {
      elements: [],
      size: 0
    }
  },
  mounted () {
    this.paginate()
    // Check for changes in props resulting from asynchronous operations
    Object.keys(this._props).forEach(event => {
      this.$watch(event, (val, oldVal) => {
        this.paginate()
      });
    })
  },
  methods: {
    add (s, f) {
      for (var i = s; i < f; i++) {
        this.elements.push(
          { type: 'page', page: i }
        )
      }
    },
    first () {
      // Add first page with separator
      this.elements.push(
        { type: 'page', page: 1 },
        { type: 'ellipse-break' }
      )
    },
    last () {
      // Add last page with separator
      this.elements.push(
        { type: 'ellipse-break' },
        { type: 'page', page: this.size },
      )
    },
    paginate () {
      this.elements = []
      this.size = Math.ceil(this.total/this.itemsPerPage)

      if (this.size < this.step * 2 + 6) {
        // Case without any ellipse breaks
        this.add(1, this.size + 1);
      }
      else if (this.current < this.step * 2 + 1) {
        // Case with ellipse breaks at end
        this.add(1, this.step * 2 + 4);
        this.last();
      }
      else if (this.current > this.size - this.step * 2) {
        // Case with ellipse breaks at beginning
        this.first();
        this.add(this.size - this.step * 2 - 2, this.size + 1);
      }
      else {
        // Case with ellipse breaks at beginning and end
        this.first();
        this.add(this.current - this.step, this.current + this.step + 1);
        this.last();
      }
    }
  }
}
</script>

<style lang="sass">
@import "~bulma/sass/utilities/_all"
@import "~bulma/sass/components/pagination"
</style>
