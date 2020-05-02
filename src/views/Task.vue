<template>
  <div class="container">
    <div v-if="task">
      <h1>Task: {{ task.title }}</h1>

      <form @submit.prevent="submitHandler" action="">
        <div class="chips center" ref="chips"></div>
        <div class="input-field col s12">
          <textarea
            v-model="description"
            id="textarea1"
            class="materialize-textarea"
          ></textarea>
          <label for="textarea1" class="center">Description</label>
          <span class="character-counter" style="float: right; font-size: 12px;"
            >{{ description.length }}/2048</span
          >
          <input type="text" ref="datepicker" />
          <div v-if="task.status != 'completed'">
            <button class="btn btn-submit" type="submit">Update Task</button>
            <hr />
            <button @click="completeTask" class="btn btn-blue">
              Complete task
            </button>
          </div>
        </div>
      </form>
    </div>
    <p v-else>Task not found</p>
  </div>
</template>
<style scoped>
textarea {
  min-height: 200px;
}
</style>
<script>
export default {
  computed: {
    task() {
      return this.$store.getters.taskById(+this.$route.params.id);
    },
  },

  name: "create",
  data: () => ({
    description: "",
    chips: null,
    date: null,
  }),
  mounted() {
    this.description = this.task.description;
    this.chips = M.Chips.init(this.$refs.chips, {
      placeholder: "Task tags",
      data: this.task.tags,
    });
    this.date = M.Datepicker.init(this.$refs.datepicker, {
      format: "dd.mm.yyyy",
      defaultDate: new Date(this.task.date),
      setDefaultDate: true,
    });
  },
  methods: {
    submitHandler() {
      this.task;
      this.$store.dispatch("updateTask", {
        id: this.task.id,
        description: this.description,
        date: this.date.date,
      });
      this.$router.push("/list");
    },
    completeTask() {
      this.$store.dispatch("completeTask", +this.task.id);
      this.$router.push("/list");
    },
  },

  dastroyed() {
    if (this.date && this.date.destroy) {
      this.date.destroy();
    }
    if (this.chips && this.chips.destroy) {
      this.chips.destroy();
    }
  },
};
</script>
