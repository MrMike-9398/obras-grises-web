<template>
  <v-form ref="form" v-model="valid" lazy-validation>
    <v-text-field
      v-model="obra.nombre_obra"
      label="Nombre del proyecto"
      required
    ></v-text-field>

    <v-text-field
      v-model="obra.descripcion"
      label="Descripcion"
      required
    ></v-text-field>

    <v-text-field v-model="obra.estado" label="Estado" required></v-text-field>

    <v-text-field
      v-model="obra.municipio"
      label="Municipio"
      required
    ></v-text-field>
    <v-file-input
      truncate-length="15"
      accept="image/*"
      label="Foto de ubicacion(opcional)"
    ></v-file-input>

    <v-file-input
      truncate-length="15"
      accept="image/*"
      label="Imagen (opcional)"
    ></v-file-input>

    <v-btn :disabled="!valid" color="success" class="mr-4" @click="createObra">
      Crear
    </v-btn>
  </v-form>
</template>

<script>
import Axios from "axios";
import Swal from 'sweetalert2'

const axios = Axios.create({
  //baseURL: "http://insane.sytes.net/mexicana-api"
  baseURL: "http://localhost:8181",
});
export default {
  data: () => ({
    obra: {},
    valid: true,
    name: "",
    nameRules: [
      (v) => !!v || "Name is required",
      (v) => (v && v.length <= 10) || "Name must be less than 10 characters",
    ],
    email: "",
    emailRules: [
      (v) => !!v || "E-mail is required",
      (v) => /.+@.+\..+/.test(v) || "E-mail must be valid",
    ],
    select: null,
    items: ["Item 1", "Item 2", "Item 3", "Item 4"],
    checkbox: false,
  }),

  methods: {
    async createObra() {
      let res = await axios.post("/obra", this.obra);
      if (res.status == 200) {
        Swal.fire("La obra se a registrado con exito");
      } else {
        Swal.fire({
          icon: "error",
          title: "Oops...",
          text: "Algo salio mal!"
          //footer: "<a href>Why do I have this issue?</a>",
        });
      }
    },
    validate() {
      this.$refs.form.validate();
    },
    reset() {
      this.$refs.form.reset();
    },
    resetValidation() {
      this.$refs.form.resetValidation();
    },
  },
};
</script>