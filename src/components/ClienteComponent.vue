<template>
  <div>
    <h2>Clientes</h2>
    <form @submit.prevent="agregarCliente">
      <input v-model="nombre" placeholder="Nombre" required />
      <input v-model="contacto" placeholder="Contacto" required />
      <button type="submit">Agregar Cliente</button>
    </form>
    <button @click="listarClientes">Listar Clientes</button>
    <ul>
      <li v-for="cliente in clientes" :key="cliente.id">
        {{ cliente.nombre }} - {{ cliente.contacto }}
        <button @click="eliminarCliente(cliente.id)">Eliminar</button>
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
      clientes: []
    };
  },
  methods: {
    async agregarCliente() {
      const response = await fetch('http://localhost:8080/clientes', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({ nombre: this.nombre, contacto: this.contacto })
      });
      if (response.ok) {
        alert('Cliente agregado correctamente');
        this.nombre = '';
        this.contacto = '';
      } else {
        alert('Error al agregar cliente');
      }
    },
    async listarClientes() {
      const response = await fetch('http://localhost:8080/clientes');
      if (response.ok) {
        this.clientes = await response.json();
      } else {
        alert('Error al listar clientes');
      }
    },
    async eliminarCliente(id) {
      const response = await fetch(`http://localhost:8080/cliente/${id}`, {
        method: 'DELETE'
      });
      if (response.ok) {
        alert('Cliente eliminado correctamente');
        this.listarClientes();
      } else {
        alert('Error al eliminar cliente');
      }
    }
  }
};
</script>
