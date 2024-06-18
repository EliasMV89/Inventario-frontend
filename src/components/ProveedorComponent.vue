<template>
    <div>
      <h2>Proveedores</h2>
      <form @submit.prevent="agregarProveedor">
        <input v-model="nombre" placeholder="Nombre" required />
        <input v-model="contacto" placeholder="Contacto" required />
        <button type="submit">Agregar Proveedor</button>
      </form>
      <button @click="listarProveedores">Listar Proveedores</button>
      <ul>
        <li v-for="proveedor in proveedores" :key="proveedor.id">
          {{ proveedor.nombre }} - {{ proveedor.contacto }}
          <button @click="eliminarProveedor(proveedor.id)">Eliminar</button>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        nombre: '',
        contacto: '',
        proveedores: []
      };
    },
    methods: {
      async agregarProveedor() {
        const response = await fetch('http://localhost:8080/proveedores', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({ nombre: this.nombre, contacto: this.contacto })
        });
        if (response.ok) {
          alert('Proveedor agregado correctamente');
          this.nombre = '';
          this.contacto = '';
        } else {
          alert('Error al agregar proveedor');
        }
      },
      async listarProveedores() {
        const response = await fetch('http://localhost:8080/proveedores');
        if (response.ok) {
          this.proveedores = await response.json();
        } else {
          alert('Error al listar proveedores');
        }
      },
      async eliminarProveedor(id) {
        const response = await fetch(`http://localhost:8080/proveedor/${id}`, {
          method: 'DELETE'
        });
        if (response.ok) {
          alert('Proveedor eliminado correctamente');
          this.listarProveedores();
        } else {
          alert('Error al eliminar proveedor');
        }
      }
    }
  };
  </script>
  