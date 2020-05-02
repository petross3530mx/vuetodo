<template>
  <nav class="nav-wrapper center blue darken-3">
    <router-link to="/create" exact active-class="active">
      Create
    </router-link>
    <router-link to="/list" active-class="active">
      List
    </router-link>
  </nav>
</template>
<style scoped>
nav {
  padding: 0 2rem;
}
nav a {
  border: 1px solid;
  padding: 4px;
  margin: 20px;
  border-radius: 2px;
}
</style>
<script>
export default {};
</script>
