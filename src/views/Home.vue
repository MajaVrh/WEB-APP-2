<template>
  <v-container 
  
    fluid
    max-width="500px"
    class="mt-16 background"
    style="width: 50% !important"
  >
    <div class="text-h5 "
     >Statistika imena</div>
    <v-col :cols="12" justify="center" align="center">
      <v-row class="inputDio">
        <v-text-field
          v-model="InputIme"
          color="purple darken-2"
          label="Upišite traženo ime"
          required
          class="w-50"
        ></v-text-field>
        <v-btn
          @click="PrikaziStatistkuImena()"
          color="purple darken-2"
          fab
          small
          dark
          class="ml-6"
        >
          <v-icon>mdi-magnify</v-icon>
        </v-btn>
      </v-row>

      <v-data-table
        :headers="headers"
        :items="PopisImena"
        class="elevation-1 mt-5"
      ></v-data-table>
    </v-col>
  </v-container>
</template>
<script>
import axios from "axios";

export default {
  //MV
  name: "Home",
  data() {
    return {
      InputIme: "",
      PopisImena: [],
      godineKorisnkika: "",
      spolKorisnika: "",
      imeKorisnika: "",
      drzavaKorisnika: "",
      headers: [
        {
          text: "Ime",
          align: "start",
          sortable: false,
          value: "ime",
        },
        { text: "Država", value: "drzava" },
        { text: "Godine", value: "godine" },
        { text: "Spol", value: "spol" },
      ],
    };
  },

  methods: {
    async PrikaziStatistkuImena() {
      const podaciGodine = await axios.get(
        "https://api.agify.io?name=" + this.InputIme
      );

      const podaciDrzava = await axios.get(
        "https://api.nationalize.io?name=" + this.InputIme
      );

      const podaciSpol = await axios.get(
        "https://api.genderize.io?name=" + this.InputIme
      );
let drz=""
    podaciDrzava.data.country.forEach(element => {
     drz += element.country_id+ " "+ "("+(element.probability*100).toFixed(0)+"%) " 
    });



      this.PopisImena.push({
        ime: this.InputIme,
        drzava: drz,
        godine: podaciGodine.data.age,
        spol: podaciSpol.data.gender + " "+"("+(podaciSpol.data.probability*100).toFixed(0) + "%) "  ,
      });
      this.InputIme=""
    },
  },
};

//Maja Vrh
</script>

<style scoped>
.text-h5{color: rgb(88, 22, 150);}

</style>
