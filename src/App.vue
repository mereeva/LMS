<template>
  <div id="app">
    <Header msg="Customer Profiles" />
    <CustomerData  @delete-user="deleteUser" />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import CustomerData from "./components/CustomerData.vue";

export default {
  name: "App",
  components: {
    Header,
    CustomerData,
  },
  data() {
    return {
      users: [],
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

</style>
