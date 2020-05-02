<template>
  <div class="row">
    <div class="col s6 offset-s3 center">
      <h1>Create task</h1>
      <form @submit.prevent="submitHandler" action="">
        <div class="input-fields">
          <label for="title">Title</label>
          <input
            v-model="title"
            id="title"
            type="text"
            class="validate"
            required
          />

          <span class="helper-text" data-error="title is required"> </span>
        </div>
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

          <button class="btn btn-submit" type="submit">Create Task</button>
        </div>
      </form>
    </div>
  </div>
</template>
<style>
.chips input {
  text-align: center;
}
</style>
<script>
export default {
  name: "create",
  data: () => ({
    description: "",
    title: "",
    chips: null,
    date: null,
  }),
  mounted() {
    this.chips = M.Chips.init(this.$refs.chips, {
      placeholder: "Task tags",
    });
    this.date = M.Datepicker.init(this.$refs.datepicker, {
      format: "dd.mm.yyyy",
      defaultDate: new Date(),
      setDefaultDate: true,
    });
  },
  methods: {
    submitHandler() {
      const task = {
        title: this.title,
        description: this.description,
        id: Date.now(),
        status: "active",
        tags: this.chips.chipsData,
        date: this.date.date,
      };

      this.$store.dispatch("createTask", task);
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
