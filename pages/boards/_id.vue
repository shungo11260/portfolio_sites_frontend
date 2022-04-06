<template>
  <div>
    {{ tasks }}
  </div>
</template>

<script>
export default {
  async asyncData({ app }) {
    const tasks = await app.$axios.$get(process.env.API_BASE + '/api/tasks')
    return { tasks }
  },
}
</script>
