<template>
  <div class="container">
    <h2>Lista de Productos</h2>

    <!-- Campo de entrada para la búsqueda por categoría -->
    <div class="input-container">
      <input v-model="categoriaBusqueda" placeholder="Buscar por categoría" />
      <button class="buscarcategory" @click="filtrarPorCategoria">Buscar</button>
    </div>

    <!-- Tabla para mostrar los productos -->
    <div class="table-responsive">
      <table class="table table-bordered text-bg-info text-white">
        <thead>
          <tr>
            <th>ID</th>
            <th @click="ordenarPor('name')">Nombre</th>
            <th>Descripción</th>
            <th>Precio</th>
            <th>Categoría</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="product in paginatedProducts" :key="product.id">
            <td>{{ product.id }}</td>
            <td>{{ product.name }}</td>
            <td>{{ product.description }}</td>
            <td>{{ product.price }} USD</td>
            <td>{{ product.category }}</td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- Contenedor de las tarjetas de producto -->
    <div class="row">
      <div v-for="product in paginatedProducts" :key="product.id" class="col-md-4 mb-4">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">{{ product.name }}</h5>
            <p class="card-text">{{ product.description }}</p>
            <p class="card-text">Precio: {{ product.price }} USD</p>
            <p class="card-text">Categoría: {{ product.category }}</p>
          </div>
        </div>
      </div>
    </div>

    <!-- Paginación -->
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
      currentPage: 1,
      productsPerPage: 12,
      categoriaBusqueda: '',
    };
  },
  computed: {
    paginatedProducts() {
      const startIndex = (this.currentPage - 1) * this.productsPerPage;
      const endIndex = startIndex + this.productsPerPage;
      return this.products.slice(startIndex, endIndex);
    },
    totalPages() {
      return Math.ceil(this.products.length / this.productsPerPage);
    },
    totalProducts() {
      return this.products.length;
    },
  },
  mounted() {
    this.obtenerProductos();
  },
  methods: {
    async obtenerProductos() {
      try {
        const response = await axios.get('https://apiproduct-nveo.onrender.com/products');
        this.products = response.data;
      } catch (error) {
        console.error('Error al obtener productos:', error);
      }
    },
    filtrarPorCategoria() {
      // Filtrar los productos por categoría
      if (this.categoriaBusqueda.trim() === '') {
        // Si la búsqueda está vacía, mostrar todos los productos
        this.obtenerProductos();
      } else {
        this.products = this.products.filter(product =>
          product.category.toLowerCase().includes(this.categoriaBusqueda.toLowerCase())
        );
      }
      this.currentPage = 1; // Reiniciar a la primera página después de filtrar
    },
    ordenarPor(columna) {
      // Implementa la lógica de ordenación si es necesario
    },
  },
};
</script>

<style scoped>
.container {
  margin-top: 20px;
}

.input-container {
  margin-bottom: 20px;
}

.card-container .card {
  width: 100%;
}

.card-title {
  font-size: 1.25rem;
  margin-bottom: .75rem;
}

.card-text {
  margin-bottom: .5rem;
}

.b-pagination {
  margin-top: 20px;
}
</style>
