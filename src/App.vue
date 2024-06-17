<template>
  <header class="header">
  <h2>DATA TABLE</h2>
  </header>

  <DataTable/>

  <footer class="footer">
    <p>&copy; 2024 Data table. All rights reserved.</p>
  </footer>
</template>

<script>
import DataTable from './components/DataTable.vue'

export default {
  name: 'App',
  components: {
    DataTable
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;

  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin-inline: 40px;

  .header {
    width: 100%;
    background-color: #2a0836;
    color: white;
    text-align: center;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 1000;
  }
  .main {
      width: 100%;
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 3.5rem 1rem; 
  }

  .footer {
      width: 100%;
      background-color: #2a0836;
      color: white;
      text-align: center;
      position: fixed;
      bottom: 0;
      left: 0;
      z-index: 1000;
  }

  .highlight {
      background-color: yellow; 
      font-weight: bold; 
  }

}
</style>
