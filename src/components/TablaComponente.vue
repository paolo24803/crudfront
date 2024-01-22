<template>
  <div>
    <table class="table">
      <thead class="table-dark">
        <tr>
          <th scope="col">Nombre</th>
          <th scope="col">Descripcion</th>
          <th scope="col">Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="cliente in clientes" :key="cliente.id">
          <td>{{ cliente.nombre }}</td>
          <td>{{ cliente.descripcion }}</td>
          <td>
            <b-button variant="success" @click="editarCliente(cliente.id_categoria)"
              >Editar</b-button
            >
            <b-button variant="danger" @click="eliminarCliente(cliente.id)"
              >Eliminar</b-button
            >
          </td>
        </tr>
      </tbody>
    </table>
    {{ ModalVisible }}
    <ModalVue
      v-if="ModalVisible"
      @hidden="ModalVisible = false"
      :paymentsData="modalData"
      :clientSelect="clientSelect"
    />
  </div>
</template>

<script>
import axios from "axios";
import ModalVue from "./ModalVue.vue";

export default {
  components: {
    ModalVue,
  },
  data() {
    return {
      clientes: [],
      payments: [],
      borrados: [],
      ModalVisible: false,
      modalData: null,
    };
  },
  mounted() {
    this.fetchClientes();
  },
  methods: {
    fetchClientes() {
      axios
        .get("http://127.0.0.1:8000/api/categorias")
        .then((response) => {
          this.clientes = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    editarCliente(clienteId) {
     // this.$emit("hidden");
      axios
        .get(`http://127.0.0.1:8000/api/buscar/${clienteId}`)
        .then((response) => {
          this.clientSelect = response.data[0];
          this.ModalVisible = true;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    // editar(){
    //   this.ModalVisible = true;      
    // },
    eliminarCliente(clienteId) {
      axios
        .post(`http://127.0.0.1:8000/api/eliminar/${clienteId}`)
        .then((response) => {
          this.clientes = response.data;
          window.location.reload();
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>
