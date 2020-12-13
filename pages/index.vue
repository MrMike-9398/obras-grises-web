<template>
  <v-card>
    <v-card-title>
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="Buscar"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>
    <v-data-table
      :headers="headers"
      :items="obras"
      :search="search"
    ></v-data-table>

  </v-card>
</template>
<script>
import Axios from "axios";

const axios = Axios.create({
  //baseURL: "http://insane.sytes.net/mexicana-api"
  baseURL: "http://localhost:8181",
});
export default {
  data() {
    return {
      obras: [],
      search: "",
      headers: [
        {
          text: "Nombre",
          align: "start",
          // filterable: false,
          value: "nombre_obra",
        },
        { text: "Estado", value: "estado" },
        { text: "Municipio", value: "municipio" },
      ],
      desserts: [
        {
          name: "Frozen Yogurt",
          calories: 159,
          fat: 6.0,
          carbs: 24,
          protein: 4.0,
          iron: "1%",
        },
        {
          name: "Ice cream sandwich",
          calories: 237,
          fat: 9.0,
          carbs: 37,
          protein: 4.3,
          iron: "1%",
        },
        {
          name: "Eclair",
          calories: 262,
          fat: 16.0,
          carbs: 23,
          protein: 6.0,
          iron: "7%",
        },
        {
          name: "Cupcake",
          calories: 305,
          fat: 3.7,
          carbs: 67,
          protein: 4.3,
          iron: "8%",
        },
        {
          name: "Gingerbread",
          calories: 356,
          fat: 16.0,
          carbs: 49,
          protein: 3.9,
          iron: "16%",
        },
        {
          name: "Jelly bean",
          calories: 375,
          fat: 0.0,
          carbs: 94,
          protein: 0.0,
          iron: "0%",
        },
        {
          name: "Lollipop",
          calories: 392,
          fat: 0.2,
          carbs: 98,
          protein: 0,
          iron: "2%",
        },
        {
          name: "Honeycomb",
          calories: 408,
          fat: 3.2,
          carbs: 87,
          protein: 6.5,
          iron: "45%",
        },
        {
          name: "Donut",
          calories: 452,
          fat: 25.0,
          carbs: 51,
          protein: 4.9,
          iron: "22%",
        },
        {
          name: "KitKat",
          calories: 518,
          fat: 26.0,
          carbs: 65,
          protein: 7,
          iron: "6%",
        },
      ],
    };
  },
  created() {
    this.initialize();
  },
  methods: {
    logout() {
      sessionStorage.clear();
      this.$router.push("/login");
    },

    async initialize() {
      let res = await axios.get("/obra");
      this.obras = res.data.object;
      console.log(res.data.object);
      //this.nombre = sessionStorage.getItem("nombre");
    },

    editItem(item) {
      this.disabledPrice = true;
      this.disabledProd = false;
      this.editedIndex = this.products.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    addprice(item) {
      this.disabledPrice = false;
      this.disabledProd = true;
      this.editedIndex = this.products.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      const index = this.products.indexOf(item);
      confirm("Are you sure you want to delete this item?") &&
        this.products.splice(index, 1);
    },

    close() {
      this.dialog = false;

      this.disabledPrice = false;
      this.disabledProd = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    async save() {
      this.disableBTNs = true;
      console.log(this.editedItem);
      let res;
      if (this.editedIndex > -1 && this.disabledPrice == true) {
        res = await axios.put(
          `/producto/${this.editedItem.idProducto}`,
          this.editedItem
        );

        if (res.status == 200) {
          this.initialize();
        } else {
          alert("Ocurrio un error");
        }
      } else if (this.editedIndex > -1 && this.disabledProd == true) {
        res = await axios.post(
          `producto/nuevoPrecio?idProducto=${this.editedItem.idProducto}&precio=${this.editedItem.precio}`
        );

        if (res.status == 200) {
          this.initialize();
        } else {
          alert("Ocurrio un error");
        }
      } else {
        res = await axios.post(
          `/producto?precio=${this.editedItem.precio}`,
          this.editedItem
        );

        if (res.status == 200) {
          this.products.push(res.data.object);
        } else {
          alert("Ocurrio un error");
        }
      }
      this.close();

      this.disabledPrice = false;
      this.disabledProd = false;
      this.disableBTNs = false;
    },
  },
};
</script>