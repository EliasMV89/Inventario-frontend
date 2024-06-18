<template>
    <div>
      <h2>Ventas</h2>
      <form @submit.prevent="agregarVenta">
        <input v-model="idProducto" type="number" placeholder="ID Producto" required />
        <input v-model="idCliente" type="number" placeholder="ID Cliente" required />
        <input v-model="cantidad" type="number" placeholder="Cantidad" required />
        <input v-model="fecha" type="date" placeholder="Fecha" required />
        <button type="submit">Agregar Venta</button>
      </form>
      <button @click="listarVentas">Listar Ventas</button>
      <ul>
        <li v-for="venta in ventas" :key="venta.id">
          {{ venta.id_producto }} - {{ venta.id_cliente }} - {{ venta.cantidad }} - {{ venta.total }} - {{ venta.fecha }}
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        idProducto: 0,
        idCliente: 0,
        cantidad: 0,
        fecha: '',
        ventas: []
      };
    },
    methods: {
      async agregarVenta() {
        const response = await fetch('http://localhost:8080/ventas', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            id_producto: this.idProducto,
            id_cliente: this.idCliente,
            cantidad: this.cantidad,
            fecha: this.fecha
          })
        });
        if (response.ok) {
          alert('Venta agregada correctamente');
          this.idProducto = 0;
          this.idCliente = 0;
          this.cantidad = 0;
          this.fecha = '';
        } else {
          alert('Error al agregar venta');
        }
      },
      async listarVentas() {
        const response = await fetch('http://localhost:8080/ventas');
        if (response.ok) {
          this.ventas = await response.json();
        } else {
          alert('Error al listar ventas');
        }
      }
    }
  };
  </script>
  