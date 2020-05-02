<template>
  <div class="container">
    <h1>List</h1>
    <div input-field col s6>
      <select ref="select" v-model="filter">
        <option value="" disabled selected>Filter</option>
        <option value="active">active</option>
        <option value="outdated">outdated</option>
        <option value="completed">completed</option>
      </select>
      <label>Filter</label>
    </div>
    <button v-if="filter" @click="filter = null" class="btn btn-small">
      Clear filter
    </button>
    <hr />
    <table v-if="tasks.length">
      <thead>
        <tr>
          <th>#</th>
          <th>Title</th>
          <th>Date</th>
          <th>Desc</th>
          <th>Status</th>
          <th>Open</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, idx) of displayTasks" :key="task.id">
          <td>{{ idx + 1 }}</td>
          <td>{{ task.title }}</td>
          <td>{{ new Date(task.date).toLocaleDateString() }}</td>
          <td class="td">
            <div class="text">{{ task.description }}</div>
          </td>
          <td>{{ task.status }}</td>
          <td>
            <router-link
              tag="button"
              class="btn btn-small"
              :to="'/task/' + task.id"
              >Open
            </router-link>
          </td>
        </tr>
      </tbody>
    </table>
    <p v-else>No tasks</p>
  </div>
</template>
<style scoped>
.text {
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
}
.td {
  max-width: 250px;
}
</style>
<script>
export default {
  data: () => ({
    filter: null,
  }),
  computed: {
    tasks() {
      return this.$store.getters.tasks;
    },
    displayTasks() {
      return this.tasks.filter((t) => {
        if (!this.filter) {
          return true;
        }
        console.log(t.status);
        console.log(this.filter);
        return t.status === this.filter;
      });
    },
  },
  mounted() {
    M.FormSelect.init(this.$refs.select, {});
  },
};
</script>
