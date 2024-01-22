<template>
  <b-modal
    id="modal-lg"
    size="lg"
    ref="ModalCategories"
    @hidden="$emit('hidden')"
    hide-footer
    title="ADD CLIENT"
  >
    <!-- Contenido del modal -->
    <!-- Formulario -->
    <form @submit="handleSubmit">
      <h3>Personal information</h3>
      <input type="hidden" id="id" v-model="client.id" /><br />
      <b-form-input
        type="text"
        id="nombre"
        v-model="client.nombre"
        placeholder="nombre"
      /><br />
      <b-form-input
        type="text"
        id="descripcion"
        v-model="client.descripcion"
        placeholder="descripcion"
      /><br />
      <div class="text-center">
        <button type="submit" variant="dark" style="width: 100%">SAVE</button>
      </div>
    </form>
  </b-modal>
</template>
<script>
import axios from "axios";

export default {
  name: "ModalVue",
  components: {},
  props: ["clientSelect"],
  data() {
    return {
      modalVisible: false,
      client: {
        id: null,
        address: null,
        name: null,
        phone: null,
        email: null,
      },
    };
  },
  methods: {
    // Función para abrir el modal
    openModal() {
      this.modalVisible = true;
    },
    handleSubmit(event) {
      event.preventDefault();
      // Crear un objeto con los datos del formulario
      const formData = {
        id: this.client.id,
        nombre: this.client.nombre,
        descripcion: this.client.descripcion,
      };

      // Enviar la solicitud HTTP POST al controlador de Laravel
      axios
        .post("http://127.0.0.1:8000/api/crearcliente", formData)
        .then((response) => {
          const client = response.data;
          console.log(client);
          this.modalVisible = false;
          this.$emit("hidden");

          window.location.reload();
        })
        .catch((error) => {
          // Manejar el error si ocurre
          console.error(error);
        });
    },
  },
  // Mostrar el modal al montar el componente
  mounted() {
    this.$nextTick(() => {
      this.$refs.ModalCategories.show();
    });

    if (this.clientSelect) {
      this.client = this.clientSelect;
    }
    if (this.paymentsSelect) {
      this.payments = this.paymentsSelect;
    }
  },
};
</script>
