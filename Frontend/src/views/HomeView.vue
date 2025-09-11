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
        <PiezaCard :pieza="pieza"  @editar="abrirModal"/>
      </div>
    </div>
  </div>
  <div v-if="mostrarModal" class="modal fade show d-block" tabindex="-1" role="dialog">
  <div class="modal-dialog" role="document">
    <div class="modal-content p-3">
      <h5 class="modal-title">Editar pieza</h5>
      <form @submit.prevent="actualizarPieza">
        <input v-model="piezaSeleccionada.nombre" class="form-control mb-2" placeholder="Nombre" />
        <textarea v-model="piezaSeleccionada.descripcion" class="form-control mb-2" placeholder="Descripción"></textarea>
        <!-- Agrega más campos si los tienes -->
        <div class="d-flex justify-content-end">
          <button type="submit" class="btn btn-success me-2">Guardar</button>
          <button type="button" class="btn btn-secondary" @click="cerrarModal">Cancelar</button>
        </div>
      </form>
    </div>
  </div>
</div>
<div v-if="mostrarModal" class="modal-backdrop fade show"></div>

</template>

<script>
import axios from 'axios';
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
      loading: true,
      mostrarModal: false,
      piezaSeleccionada: null
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
  },
  methods: {
    abrirModal(pieza) {
      this.piezaSeleccionada = { ...pieza };
    this.mostrarModal = true;
    },
    cerrarModal() {
        this.mostrarModal = false;
    this.piezaSeleccionada = null;
    },
    async actualizarPieza() {
      try {
      const response = await axios.put(`http://localhost:3000/api/control_interno/${this.piezaSeleccionada._id}`, this.piezaSeleccionada);
      const index = this.piezas.findIndex(p => p._id === this.piezaSeleccionada._id);
      this.piezas[index] = response.data;
      this.cerrarModal();
      alert('✅ Pieza actualizada correctamente');
    } catch (error) {
      console.error('❌ Error al actualizar:', error);
      alert('Hubo un problema al guardar los cambios');
    }
    }
  }
};
</script>
