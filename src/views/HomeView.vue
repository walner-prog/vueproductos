<template>
  <div>
    <h1 class="mb-4">Mi Aplicación Vue.js para Mostrar Datos de Productos</h1>
    <p>Soy un apasionado desarrollador web y esta es una aplicación construida con Vue.js. Aquí estoy mostrando datos de productos en una tabla interactiva a través del consumo de una API que desarrollé con Node.js y Express.js.</p>
    
    <!-- Contenedor de las tarjetas de producto -->
    <div class="row">
      <div v-for="product in paginatedProducts" :key="product.id" class="col-md-4 mb-4">
        <div class="card h-100 shadow">
          <div class="card-body">
            <h5 class="card-title text-primary">{{ product.name }}</h5>
            <p class="card-text">{{ product.description }}</p>
            <p class="card-text text-success">Precio: {{ product.price }} USD</p>
            <p class="card-text text-info">Categoría: {{ product.category }}</p>
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
      class="mt-4"
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
  },
};
</script>

<style scoped>
.card {
  border-radius: 15px;
  transition: all 0.3s ease;
}

.card:hover {
  transform: translateY(-5px);
}

.card-title {
  font-size: 1.5rem;
}

.card-text {
  font-size: 1.1rem;
}

.b-pagination {
  justify-content: center;
}
</style>
