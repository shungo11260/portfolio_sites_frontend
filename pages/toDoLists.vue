<template>
  <div>
    {{ toDos }}
  </div>
</template>

<script>
export default {
  async asyncData({ app }) {
    const toDos = await app.$axios.$get(process.env.API_BASE + '/api/to_dos')
    return { toDos }
  },
}
</script>
