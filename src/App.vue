<template>
  <div id="app">
    <ListStudent/>
<!--    <employees />-->
  </div>
</template>
<script>
import ListStudent from "./components/ListStudent";
// import Employees from "./components/Employees";
export default {
  name: 'App',
  components: {
    ListStudent
    // Employees
  },
  data(){
    return {
      users: [],
      message: 'this is text'
    }
  },
}
</script>

<style>

</style>
