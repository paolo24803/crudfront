<template>
  <div>
    <b-card name="card">
      <div class="d-flex justify-content-between mb-4">
        <titulo-vue />
        <b-button @click="openModal()" variant="primary" class="btn btn-success"
          >ADD CLIENT</b-button
        >
        <!--Declarar variable y emitir modal -->
        <ModalVue v-if="ModalVisible" @hidden="ModalVisible = false" />
      </div>
      <TablaComponente />
    </b-card>
  </div>
</template>

<script>
import TablaComponente from "@/components/TablaComponente.vue";
import "bootstrap/dist/css/bootstrap.css";
import TituloVue from "@/components/TituloVue.vue";
import ModalVue from "@/components/ModalVue.vue";

export default {
  name: "App",
  components: {
    TablaComponente,
    TituloVue,
    ModalVue,
  },
  methods: {
    openModal() {
      this.ModalVisible = true;
    },
  },
  data() {
    return {
      ModalVisible: false,
    };
  },
};
</script>