<template>
  <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/about">Lista de productos</router-link>
  </nav>
  <router-view/>

  <div id="app" class="container mt-5">
    

  </div>

  
</template>


<script>
import axios from 'axios';

export default {
  data() {
    return {
      libros: [],
    };
  },
  mounted() {
    // Hacer una solicitud GET a la API
    axios.get('http://localhost:3000/libros')
      .then(response => {
        // Manejar los datos obtenidos
        this.libros = response.data.libros;
      })
      .catch(error => {
        // Manejar errores
        console.error('Error al obtener libros:', error);
      });
  },
};
</script>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  
  
}

.container thead{
  background-color: darkslategray;
  color: darkgreen;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
.tr-encabezado{
  background-color: darkslategray;
}
</style>
