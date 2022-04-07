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
            <v-btn @click="reveal = true">
              <v-icon>mdi-plus</v-icon>
              add one more list
            </v-btn>
          </v-card-actions>
          <v-expand-transition>
            <v-card
              v-if="reveal"
              v-click-outside="onClickOutsideForm"
              class="fade-transition v-card-reveal"
            >
              <v-form>
                <v-text-field v-model="model" label="title"></v-text-field>
                <v-btn type="submit"> add list </v-btn>
              </v-form>
            </v-card>
          </v-expand-transition>
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
  data: () => ({
    reveal: false,
  }),
  methods: {
    onClickOutsideForm() {
      this.reveal = false
    },
    // async submit() {
    //   const response = await app.$axios.$
    // },
  },
}
</script>

<style>
.v-card-reveal {
  top: 0;
  opacity: 1 !important;
  position: absolute;
  width: 100%;
}
</style>
