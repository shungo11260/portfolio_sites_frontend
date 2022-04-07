<template>
  <v-container>
    <v-row>
      <v-col v-for="list in result.lists" :key="list.id">
        <v-card elevation="6">
          <v-card-title>{{ list.title }}</v-card-title>
        </v-card>
      </v-col>
      <v-col>
        <v-card elevation="6">
          <v-card-actions>
            <v-btn>
              <v-icon>mdi-plus</v-icon>
              add list
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  async asyncData({ app, params }) {
    const result = await app.$axios.$get(
      process.env.API_BASE + '/api/boards/' + params.id
    )
    return { result }
  },
}
</script>
