<template>
  <li>
    <a
      :class="['pagination-link', { 'is-info': current === page }]"
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
<style scoped>
  .pagination-link.is-info {
    background-color: #3e8ed0;
    border-color: #3e8ed0;
    color: #fff;
  }
</style>
