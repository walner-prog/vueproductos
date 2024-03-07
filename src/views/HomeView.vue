<template>
  <div id="app" class="container mt-5">
    <h1 class="mb-4">Lista de Libros</h1>
    <!--<ul class="list-group">
      <li v-for="libro in libros" :key="libro.id" class="list-group-item">
        {{ libro.titulo }} - {{ libro.autor }}
      </li>
    </ul>-->

       <table class="table table-bordered table-striped ">

        <thead  >
          <tr class="tr-encabezado" style="color:brown">
            <th>ID</th>
            <th>TITULO</th>
           
          </tr>
        </thead>
        <tbody>
        
          <tr v-for="lenguaje in Lenguajes" :key="lenguaje.id">
            <td>{{ lenguaje.id }}</td>
            <td>{{ lenguaje.titulo }} </td>
           
          </tr>
          
         </tbody>
       </table>
  </div>

  
</template>


<script>
import axios from 'axios';

export default {
  data() {
    return {
      Lenguajes: [],
    };
  },
  mounted() {
    // Hacer una solicitud GET a la API
    axios.get('http://localhost:3000/Lenguajes')
      .then(response => {
        // Manejar los datos obtenidos
        this.Lenguajes = response.data.Lenguajes;
      })
      .catch(error => {
        // Manejar errores
        console.error('Error al obtener Lenguaje:', error);
      });
  },
};
</script>

