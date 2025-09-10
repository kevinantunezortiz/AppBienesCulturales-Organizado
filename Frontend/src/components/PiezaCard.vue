<template>
  <div class="card h-100 shadow-sm">
    <img
      v-if="pieza.url_imagenes?.length"
      :src="pieza.url_imagenes[0]"
      class="card-img-top img-fluid"
      alt="Imagen de la pieza"
    />
    <div class="card-body">
      <h5 class="card-title">{{ pieza.nombre }}</h5>
      <p class="card-text">
        <strong>Cultura:</strong> {{ pieza.cultura }}<br />
        <strong>Ubicación:</strong> {{ pieza.ubicacion }}
      </p>
      <router-link :to="`/detalle/${pieza._id}`" class="btn btn-outline-primary btn-sm">
        Ver detalles
      </router-link>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PiezaCard',
  props: {
    pieza: {
      type: Object,
      required: true
    }
  }
};
</script>
