<template>
  <div>
    {{ toDos }}
  </div>
</template>

<script>
export default {
  async asyncData({ app }) {
    const toDos = await app.$axios.$get('http://localhost:8080/api/to_dos')
    return { toDos }
  },
}
</script>
