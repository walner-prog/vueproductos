<!-- src/components/ProductList.vue -->

<template>
  <head>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0/dist/css/bootstrap.min.css">
  <!-- BootstrapVue CSS via CDN -->

 <link rel="stylesheet" href="../assets/bootstrap.css">

  </head>
  <div class=" container">
    <h2>Lista de Productos</h2>
    <!-- Agregar un campo de entrada para la búsqueda por categoría -->
<input v-model="categoriaBusqueda" placeholder="Buscar por categoría " value="category" >
<button class="buscarcategory" @click="filtrarPorCategoria">Buscar</button>

<input v-model="name" placeholder="Buscar por nombre del producto " value="category">
<button @click="filtrarProductosPorNombre">Buscar</button>
     
    <table class="table table-bordered text-bg-info text-white table-responsive">
      <thead>
        <tr class=" text-warning">
          <th>ID</th>
          <th @click="ordenarPor('name')">Nombre</th>
          <th>Descripción</th>
          <th @click="ordenarPor('price')">Precio</th>
          <th>Categoría</th>
          <th>Imagen</th>
         
        </tr>
      </thead>
      <tbody>
        <tr v-for="product in products" :key="product.id">
          <td>{{ product.id }}</td>
          <td>{{ product.name }}</td>
          <td>{{ product.description }}</td>
          <td>{{ product.price }} USD</td>
          <td>{{ product.category }}</td>
          <td>
            <img :src="product.image" alt="Product Image" style="max-width: 50px; max-height: 50px;">
          </td>
          <!--<td>
            <button @click="actualizarProducto(product.id, updatedProduct)">Actualizar</button>
            <button @click="eliminarProducto(product.id)">Eliminar</button>
          </td>-->
        </tr>
      </tbody>
    </table>

    <b-pagination
    v-if="totalPages > 1"
    v-model="currentPage"
    :total-rows="totalProducts"
    :per-page="productsPerPage"
    aria-controls="my-table"
  ></b-pagination>
  </div>
</template>

<script>

import axios from 'axios';

export default {
  
  data() {
    return {
      products: [],
      
      orden: { columna: null, direccion: 1 }, // Inicialmente, no hay orden

      categoriaBusqueda: '', // Inicialmente, la búsqueda está vacía
    };

    
  },
  mounted() {

    this.obtenerProductos();

    // Hacer la solicitud para obtener todos los productos al cargar el componente
    axios.get('http://localhost:3000/products')
      .then(response => {
        this.products = response.data;
      })
      .catch(error => {
        console.error('Error:', error);
      });
  },
  methods: {
// Optener toods los productos
    async obtenerProductos() {
      try {
        const response = await axios.get('http://localhost:3000/products');
        this.productos = response.data;
        this.totalProducts = this.productos.length;
      } catch (error) {
        console.error('Error al obtener productos:', error);
      }
    },
      
    // aptualizar  un  producto
    async actualizarProducto(productId, updatedProduct) {
      try {
        const response = await axios.put(`http://localhost:3000/products/${productId}`, updatedProduct);
        console.log(response.data); // Aquí puedes manejar la respuesta de la API
      } catch (error) {
        console.error('Error al actualizar el producto', error);
      }
    },

    mostrarFormularioActualizar(producto) {
      // Aquí puedes abrir un formulario modal o navegar a otra página para actualizar el producto
      console.log('Mostrar formulario para actualizar:', producto);
    },

     // Eliminar  un  producto
    async eliminarProducto(productId) {
      try {
        await axios.delete(`http://localhost:3000/products/${productId}`);
        this.obtenerProductos();
      } catch (error) {
        console.error('Error al eliminar el producto:', error);
      }
    },
    async filtrarProductosPorNombre(name) {
      try {
        const response = await axios.get(`http://localhost:3000/products/filter/name?name=${name}`);
        console.log(response.data); // Aquí puedes manejar la respuesta de la API
         // Filtrar los productos por categoría
         this.products = this.products.filter(product =>
        product.name.toLowerCase().includes(this.name.toLowerCase())
      );
      this.totalProducts = this.products.length;
      } catch (error) {
        console.error('Error al filtrar productos por nombre', error);
      }
    },

    ordenarPor(columna) {
    // Cambiar la columna de orden si ya está ordenando por ella, de lo contrario, establecer la nueva columna
    this.orden.columna = this.orden.columna === columna ? null : columna;
    // Cambiar la dirección de orden
    this.orden.direccion = this.orden.direccion === 1 ? -1 : 1;

    // Ordenar los productos
    if (this.orden.columna) {
      this.products.sort((a, b) => {
        const comparison = a[this.orden.columna].localeCompare(b[this.orden.columna]);
        return this.orden.direccion * comparison;
      });
    }
  },

  filtrarPorCategoria() {
    if (this.categoriaBusqueda.trim() === '') {
      // Si la búsqueda está vacía, muestra todos los productos
     // this.fetchProducts();
      this. obtenerProductos();
    } else {
      // Filtrar los productos por categoría
      this.products = this.products.filter(product =>
        product.category.toLowerCase().includes(this.categoriaBusqueda.toLowerCase())
      );
      this.totalProducts = this.products.length;
    }
    this. obtenerProductos();
  },
 
  },
  

};



</script>


<style scoped>
/* Estilos específicos del componente si es necesario */
table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
  color:#4d4343
}

th {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
  color:#FFFFFF
}

th {
  background-color: #39CD39;
}

button {
  border-radius: 0;
  background-color: rgb(162, 162, 255);
}

button:focus:not(:focus-visible) {
  outline: 0;
}

input,
button,
select,
optgroup,
textarea {
  margin: 0;
  font-family: inherit;
  font-size: inherit;
  line-height: inherit;
}

button,
input {
  overflow: visible;
}

button,
select {
  text-transform: none;
}

[role="button"] {
  cursor: pointer;
}

select {
  word-wrap: normal;
}

button,
[type="button"],
[type="reset"],
[type="submit"] {
  -webkit-appearance: button;
}

button:not(:disabled),
[type="button"]:not(:disabled),
[type="reset"]:not(:disabled),
[type="submit"]:not(:disabled) {
  cursor: pointer;
}

button::-moz-focus-inner,
[type="button"]::-moz-focus-inner,
[type="reset"]::-moz-focus-inner,
[type="submit"]::-moz-focus-inner {
  padding: 0;
  border-style: none;
}

input[type="radio"],
input[type="checkbox"] {
  box-sizing: border-box;
  padding: 0;
}
.buscarcategory{
  margin-right: 10px;
}

</style>
