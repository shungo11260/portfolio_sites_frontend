<template>
  <div>
    {{ board }}
  </div>
</template>

<script>
export default {
  async asyncData({ app, params }) {
    const board = await app.$axios.$get(
      process.env.API_BASE + '/api/boards/' + params.id
    )
    return { board }
  },
}
</script>
