<template>
  <v-container>
    <v-card v-for="board in boards" :key="board.id" :to="'/boards/' + board.id">
      <v-card-title>{{ board.title }}</v-card-title>
    </v-card>
  </v-container>
</template>

<script>
export default {
  name: 'BoardsPage',
  async asyncData({ app }) {
    const boards = await app.$axios.$get(process.env.API_BASE + '/api/boards')
    return { boards }
  },
}
</script>
