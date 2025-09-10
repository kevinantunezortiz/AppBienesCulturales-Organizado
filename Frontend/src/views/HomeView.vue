<template>
  <div class="container mt-4">
    <div class="d-flex justify-content-between align-items-center mb-3">
      <h2>Piezas registradas</h2>
      <router-link to="/registro" class="btn btn-success">Registrar nueva pieza</router-link>
    </div>

    <div v-if="loading" class="text-center my-5">
      <div class="spinner-border text-primary" role="status"></div>
    </div>

    <div v-else-if="piezas.length === 0" class="alert alert-info">
      No hay piezas registradas aún.
    </div>

    <div v-else class="row row-cols-1 row-cols-md-2 row-cols-lg-3 g-4">
      <div class="col" v-for="pieza in piezas" :key="pieza._id">
        <PiezaCard :pieza="pieza" />
      </div>
    </div>
  </div>
</template>

<script>
import { getRegistros } from '../services/api';
import PiezaCard from '../components/PiezaCard.vue';

export default {
  name: 'HomeView',
  components: {
    PiezaCard
  },
  data() {
    return {
      piezas: [],
      loading: true
    };
  },
  async mounted() {
    try {
      const response = await getRegistros();
      this.piezas = response.data;
    } catch (error) {
      console.error('Error al obtener piezas:', error);
    } finally {
      this.loading = false;
    }
  }
};
</script>
