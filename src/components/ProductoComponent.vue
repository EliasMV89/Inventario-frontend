<template>
    <div>
      <h2>Productos</h2>
      <form @submit.prevent="agregarProducto">
        <input v-model="nombre" placeholder="Nombre" required />
        <input v-model="categoria" placeholder="Categoría" required />
        <input v-model="precio" type="number" step="0.01" placeholder="Precio" required />
        <input v-model="cantidad" type="number" placeholder="Cantidad" required />
        <input v-model="idProveedor" type="number" placeholder="ID Proveedor" required />
        <button type="submit">Agregar Producto</button>
      </form>
      <button @click="listarProductos">Listar Productos</button>
      <ul>
        <li v-for="producto in productos" :key="producto.id">
          {{ producto.nombre }} - {{ producto.categoria }} - {{ producto.precio }} - {{ producto.cantidad }}
          <button @click="eliminarProducto(producto.id)">Eliminar</button>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        nombre: '',
        categoria: '',
        precio: 0,
        cantidad: 0,
        idProveedor: 0,
        productos: []
      };
    },
    methods: {
      async agregarProducto() {
        const response = await fetch('http://localhost:8080/productos', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            nombre: this.nombre,
            categoria: this.categoria,
            precio: this.precio,
            cantidad: this.cantidad,
            id_proveedor: this.idProveedor
          })
        });
        if (response.ok) {
          alert('Producto agregado correctamente');
          this.nombre = '';
          this.categoria = '';
          this.precio = 0;
          this.cantidad = 0;
          this.idProveedor = 0;
        } else {
          alert('Error al agregar producto');
        }
      },
      async listarProductos() {
        const response = await fetch('http://localhost:8080/productos');
        if (response.ok) {
          this.productos = await response.json();
        } else {
          alert('Error al listar productos');
        }
      },
      async eliminarProducto(id) {
        const response = await fetch(`http://localhost:8080/producto/${id}`, {
          method: 'DELETE'
        });
        if (response.ok) {
          alert('Producto eliminado correctamente');
          this.listarProductos();
        } else {
          alert('Error al eliminar producto');
        }
      }
    }
  };
  </script>
  