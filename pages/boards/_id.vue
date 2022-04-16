<template>
  <v-container>
    <v-row>
      <v-col v-for="list in result.lists" :key="list.id" :ref="'lists'">
        <v-card elevation="6">
          <v-card-title>{{ list.title }}</v-card-title>
          <v-card
            v-for="task in result.tasks.filter(
              (task) => task.list_id === list.id
            )"
            :key="task.id"
            elevation="6"
          >
            <v-card-title>{{ task.title }}</v-card-title>
          </v-card>
          <v-card-actions>
            <v-btn @click="reveal = list.id.toString()">
              <v-icon>mdi-plus</v-icon>
              add task
            </v-btn>
          </v-card-actions>
          <v-expand-transition>
            <v-card
              v-if="reveal === list.id.toString()"
              v-click-outside="{
                handler: onClickOutsideForm,
                include: cards,
              }"
              class="fade-transition"
            >
              <v-form @submit.prevent="onTaskSubmit(list.id)">
                <v-text-field
                  v-model="newTask.task.title"
                  label="title"
                ></v-text-field>
                <v-btn type="submit"> add task </v-btn>
              </v-form>
            </v-card>
          </v-expand-transition>
        </v-card>
      </v-col>
      <v-col :ref="'addList'">
        <v-card elevation="6">
          <v-card-actions>
            <v-btn @click="reveal = '0'">
              <v-icon>mdi-plus</v-icon>
              add one more list
            </v-btn>
          </v-card-actions>
          <v-expand-transition>
            <v-card
              v-if="reveal === '0'"
              v-click-outside="{
                handler: onClickOutsideForm,
                include: cards,
              }"
              class="fade-transition v-card-reveal"
            >
              <v-form @submit.prevent="onSubmit">
                <v-text-field
                  v-model="newList.list.title"
                  label="title"
                ></v-text-field>
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
    reveal: '',
    newList: {
      list: {
        title: '',
        board_id: '',
      },
    },
    newTask: {
      task: {
        title: '',
        list_id: '',
      },
    },
  }),
  mounted() {
    this.newList.list.board_id = this.$route.params.id
  },
  methods: {
    onClickOutsideForm() {
      this.reveal = ''
    },
    cards() {
      const cardElements = Object.values(this.$refs.lists).map(
        (element) => element.firstElementChild.lastElementChild
      )
      cardElements.push(this.$refs.addList.firstElementChild.lastElementChild)
      return cardElements
    },
    async onSubmit() {
      await this.$axios.$post(process.env.API_BASE + '/api/lists', this.newList)
      this.$nuxt.refresh()
    },
    async onTaskSubmit(listId) {
      this.newTask.task.list_id = listId
      await this.$axios.$post(process.env.API_BASE + '/api/tasks', this.newTask)
      this.$nuxt.refresh()
    },
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
