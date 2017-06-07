<template>
  <li>
    <a :class="['pagination-link', { 'is-current': current === page }]"
        @click="onChange(page)">
        {{ page }}
    </a>
  </li>
</template>

<script>
export default {
  name: 'page',
  props: ['page', 'current', 'onChange']
}
</script>
