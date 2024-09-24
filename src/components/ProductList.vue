<script setup>
import { ref, computed, onMounted } from 'vue';

// Variables de estado
const productos = ref([]);
const buscarproductos = ref('');
const verproductos = ref(10);

// Función para buscar productos desde la API
const buscarProductos = async () => {
  const response = await fetch('https://fakestoreapi.com/products');
  const data = await response.json();
  productos.value = data;
};

// Función para cargar más productos
const verMas = () => {
  verproductos.value += 10;
};

// Computed para filtrar productos
const filtroProductos = computed(() => {
  return productos.value.filter(producto =>
    producto.title.toLowerCase().includes(buscarproductos.value.toLowerCase())
  );
});

// Computed para mostrar los productos visibles según la cantidad
const verFiltroProductos = computed(() => {
  return filtroProductos.value.slice(0, verproductos.value);
});

// Ejecutar la búsqueda de productos cuando el componente es montado
onMounted(() => {
  buscarProductos();
});
</script>
<template>
    <div class="container">
      <input
        v-model="buscarproductos"
        placeholder="Buscar productos..."
        class="search-bar"
      />
  
      <div class="grid">
        <div v-for="producto in verFiltroProductos" :key="producto.id" class="card">
          <div class="image-container">
            <img :src="producto.image" alt="Imagen del producto" class="product-image" />
          </div>
          <h3>{{ producto.title }}</h3>
          <p class="description">{{ producto.description }}</p>
          <p class="price">Precio: ${{ producto.price }}</p>
        </div>
      </div>
  
      <button
        v-if="verFiltroProductos.length < filtroProductos.length"
        @click="verMas"
        class="load-more-btn"
      >
        Ver más
      </button>
    </div>
  </template>
  
  <style scoped>
  /* Estilos generales */
  .container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
    font-family: 'Roboto', sans-serif;
    background-color: #f7f7f7;
    border-radius: 10px;
  }
  
  /* Barra de búsqueda estilizada */
  .search-bar {
    width: 100%;
    padding: 12px;
    margin-bottom: 20px;
    border: 2px solid #e0e0e0;
    border-radius: 10px;
    font-size: 16px;
    background-color: #fff;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    transition: border-color 0.3s ease;
  }
  
  .search-bar:focus {
    border-color: #3498db;
  }
  
  /* Estilos para la cuadrícula */
  .grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
    gap: 30px;
  }
  
  .card {
    background-color: #fff;
    border-radius: 15px;
    padding: 20px;
    box-shadow: 0 8px 15px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    text-align: center;
  }
  
  .card:hover {
    transform: scale(1.05);
    box-shadow: 0 12px 20px rgba(0, 0, 0, 0.2);
  }
  
  .image-container {
    height: 180px;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 15px;
  }
  
  .product-image {
    max-width: 100%;
    max-height: 100%;
    border-radius: 10px;
  }
  
  .card h3 {
    font-size: 1.2em;
    margin-bottom: 10px;
    color: #333;
  }
  
  .description {
    font-size: 0.9em;
    margin-bottom: 10px;
    color: #666;
  }
  
  .price {
    font-size: 1.1em;
    font-weight: bold;
    color: #27ae60;
  }
  
  /* Botón para cargar más productos */
  .load-more-btn {
    background-color: #3498db;
    color: white;
    padding: 14px 25px;
    border: none;
    border-radius: 12px;
    font-size: 16px;
    cursor: pointer;
    transition: background-color 0.3s ease, transform 0.2s;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
    margin-top: 30px;
  }
  
  .load-more-btn:hover {
    background-color: #2980b9;
    transform: translateY(-2px);
  }
  
  /* Responsivo para pantallas más pequeñas */
  @media (max-width: 768px) {
    .grid {
      grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
      gap: 15px;
    }
  
    .card {
      padding: 15px;
    }
  
    .search-bar {
      font-size: 14px;
    }
  
    .load-more-btn {
      width: 100%;
    }
  }
  </style>
  