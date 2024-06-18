<template>
  <div id="app">
    <h1>Inventario</h1>
    <ClienteComponent />
    <ProductoComponent />
    <ProveedorComponent />
    <VentaComponent />
  </div>
</template>

<script>
import ClienteComponent from './components/ClienteComponent.vue';
import ProductoComponent from './components/ProductoComponent.vue';
import ProveedorComponent from './components/ProveedorComponent.vue';
import VentaComponent from './components/VentaComponent.vue';

export default {
  name: 'App',
  components: {
    ClienteComponent,
    ProductoComponent,
    ProveedorComponent,
    VentaComponent
  }
};
</script>
