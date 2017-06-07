<template>
  <div id="pagination">
    <nav class="pagination is-centered">
      <a class="pagination-previous">&larr; Previous</a>
      <a class="pagination-next">Next &rarr;</a>
      <ul class="pagination-list">

        <template v-for="block in blocks">
          <template v-if="block.type === 'ellipse'">
            <li><span class="pagination-ellipsis">&hellip;</span></li>
          </template>
          <template v-else>
            <li>
              <a :class="['pagination-link', {'is-current': block.active}]"
                  @click="onChange(block.page)">
                  {{ block.page }}
              </a>
            </li>
          </template>
        </template>

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
    return {
      blocks: []
    }
  },
  mounted () {
    // Check for changes in props resulting from asynchronous operations
    Object.keys(this._props).forEach(event => {
      this.$watch(event, (val, oldVal) => {
        this.createBlocks()
      });
    })
  },
  methods: {
    createBlocks () {
      this.blocks = []
      console.log('Creating pagination blocks.')
      let pages = Math.floor(this.total/this.itemsPerPage)
      console.log(`# of Pages: ${pages}`)
      console.log(`Current: ${this.current}`)
      for (let i = 1; i < pages + 1; i++) {
        let active = this.current === i ? true : false
        this.blocks.push({type: 'page', page: i, active: active})
      }
      console.log(`Blocks: ${this.blocks}`)

      if (pages > 5 && this.current <= 3) {
        // Add ellipse towards the end
        this.blocks.splice(-1, 0, { type: 'ellipse' })
      } else if (pages > 6 && this.current > 3 && this.current <= pages - 3) {
        // Add ellipse at front and towards end
        this.blocks.splice(1, 0, { type: 'ellipse' })
        this.blocks.splice(-1, 0, { type: 'ellipse' })
      } else if (pages > 5 && this.current > 3) {
        // Add ellipse at front
        this.blocks.splice(1, 0, { type: 'ellipse' })
      }
    }
  }
}
</script>

<style lang="sass" scoped>
@import "../node_modules/bulma/sass/utilities/_all"
@import "../node_modules/bulma/sass/components/pagination"
</style>
