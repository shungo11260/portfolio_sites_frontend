<template>
  <div>
    {{ boards }}
  </div>
</template>

<script>
export default {
  async asyncData({ app }) {
    const boards = await app.$axios.$get(process.env.API_BASE + '/api/boards')
    return { boards }
  },
}
</script>
