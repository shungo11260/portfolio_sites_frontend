<template>
  <v-container>
    <v-card v-for="list in res.lists" :key="list.id">
      <v-card-title>{{ list.title }}</v-card-title>
    </v-card>
  </v-container>
</template>

<script>
export default {
  async asyncData({ app, params }) {
    const res = await app.$axios.$get(
      process.env.API_BASE + '/api/boards/' + params.id
    )
    return { res }
  },
}
</script>
