<template>
<div>
 <container>
<v-row>
        <v-col  md="3">
            
            <v-card
                class="mx-auto"
                max-width="280"
                shaped
            >
                <v-list-item two-line>
                <v-list-item-content  >
                    <v-list-item-title class="text-h6 mb-1">
                        <h3> {{ habitacionesVisualizar }} </h3>
                    </v-list-item-title>
                    <v-list-item-subtitle> Habitaciones disponibles </v-list-item-subtitle>
                </v-list-item-content>
                <v-icon
                color="green"
                size="50px"
                >
                    mdi-bed  
                </v-icon>
                </v-list-item>
            </v-card>
        </v-col>

        <v-col  md="3">
           
            <v-card
                class="mx-auto"
                max-width="280"
                shaped
            >
                <v-list-item two-line>
                <v-list-item-content>
                    <v-list-item-title class="text-h6 mb-1" >
                        <h3> {{ habitacionesVisualizar1 }} </h3>
                    </v-list-item-title>
                    <v-list-item-subtitle> Habitaciones ocupadas </v-list-item-subtitle>
                </v-list-item-content>
                <v-icon
                color="red"
                size="50px"
                >
                    mdi-bed  
                </v-icon>
                </v-list-item>
            </v-card>
        </v-col>

        <v-col  md="3">
            
            <v-card
                class="mx-auto"
                max-width="280"
                shaped
            >
                <v-list-item two-line>
                <v-list-item-content>
                    <v-list-item-title class="text-h6 mb-1">
                        <h3> {{ habitacionesVisualizar2 }} </h3>
                    </v-list-item-title>
                    <v-list-item-subtitle> Habitaciones reservadas </v-list-item-subtitle>
                </v-list-item-content>
                <v-icon
                color="yellow"
                size="50px"
                >
                    mdi-bed  
                </v-icon>
                </v-list-item>
            </v-card>
        </v-col>     
           
        <v-col  md="3">
            
            <v-card
                class="mx-auto"
                max-width="280"
                shaped
            >
                <v-list-item two-line>
                <v-list-item-content>
                    <v-list-item-title class="text-h6 mb-1"> 
                        <h3> {{ habitacionesVisualizar3 }} </h3>
                    </v-list-item-title>
                    <v-list-item-subtitle> Habitaciones en limpieza </v-list-item-subtitle>
                </v-list-item-content>
                <v-icon
                color="blue"
                size="50px"
                >
                    mdi-bed  
                </v-icon>
                </v-list-item>
            </v-card>
        </v-col>
       

        <v-col  md="4">
           
            <v-card
                class="mx-auto"
                max-width="280"
                outline
            >
                <v-list-item two-line>
                <v-list-item-content>
                    <v-list-item-title class="text-h6 mb-1">
                      {{ reservaVisualizar }}
                    </v-list-item-title>
                    <v-list-item-subtitle> Reservas</v-list-item-subtitle>
                </v-list-item-content>
                <v-icon
                color="gray"
                size="50px"
                >
                    mdi-bell 
                </v-icon>
                </v-list-item>
            </v-card>
        </v-col>

        <v-col  md="4">
            
            <v-card
                class="mx-auto"
                max-width="280"
                outline
            >
                <v-list-item two-line>
                <v-list-item-content>
                    <v-list-item-title class="text-h6 mb-1">
                    {{ reservaVisualizar1 }}
                    </v-list-item-title>
                    <v-list-item-subtitle> Check-In </v-list-item-subtitle>
                </v-list-item-content>
                <v-icon
                color="gray"
                size="50px"
                >
                    mdi-bell
                </v-icon>
                </v-list-item>
            </v-card>
        </v-col>

        <v-col  md="4">
           
            <v-card
                class="mx-auto"
                max-width="280"
                outline
            >
                <v-list-item two-line>
                <v-list-item-content>
                    <v-list-item-title class="text-h6 mb-1">
                    {{ reservaVisualizar2 }}
                    </v-list-item-title>
                    <v-list-item-subtitle> Check-Out </v-list-item-subtitle>
                </v-list-item-content>
                <v-icon
                color="gray"
                size="50px"
                >
                    mdi-bell 
                </v-icon>
                </v-list-item>
            </v-card>
        </v-col>
    </v-row>
</container>
</div>
   
</template>


<script>
import axios from "axios";
export default {
  data: () => ({
      
    reservaVisualizar: [], 
    reservaVisualizar1: [],
    reservaVisualizar2: [],
    habitacionesVisualizar: [],
    habitacionesVisualizar1: [],
    habitacionesVisualizar2: [],
    habitacionesVisualizar3: [],

    estado: [
      {
        nombre: "Reservadas"
      },
      {
        nombre: "Limpieza"
      },
      {
        nombre: "Ocupada"
      },
      {
        nombre: "Disponible"
      }
    ],
    
  }),

  mounted () {
      this.$refs.calendar.checkChange()
    },

  created() {
    this.obtenerReservas();
    this.obtenerReservas1();
    this.obtenerReservas2();
    this.obtenerHabitaciones();
    this.obtenerHabitaciones1();
    this.obtenerHabitaciones2();
    this.obtenerHabitaciones3();
  },

  methods: {

    obtenerHabitaciones: async function () {
      await axios.get("http://localhost:3000/habitaciones/").then((resp) => {
        if (resp.status == 200) {
          this.habitacionesVisualizar = resp.data.length;
          console.log('------------------------------------------');
          console.log(resp.data.length);

        }
      });
    },

    obtenerHabitaciones1: async function () {
      await axios.get("http://localhost:3000/habitaciones/list1").then((resp) => {
        if (resp.status == 200) {
          this.habitacionesVisualizar1 = resp.data.length;
          console.log('------------------------------------------');
          console.log(resp.data.length);

        }
      });
    },

    obtenerHabitaciones2: async function () {
      await axios.get("http://localhost:3000/habitaciones/list2").then((resp) => {
        if (resp.status == 200) {
          this.habitacionesVisualizar2 = resp.data.length;
          console.log('------------------------------------------');
          console.log(resp.data.length);

        }
      });
    },

    obtenerHabitaciones3: async function () {
      await axios.get("http://localhost:3000/habitaciones/list3").then((resp) => {
        if (resp.status == 200) {
          this.habitacionesVisualizar3 = resp.data.length;
          console.log('------------------------------------------');
          console.log(resp.data.length);

        }
      });
    },

    obtenerReservas: async function () {
      console.log("Hola uuuu");
      await axios.get("http://localhost:3000/reserva/list1").then((resp) => {
        if (resp.status == 200) {
          this.reservaVisualizar = resp.data.length;
          console.log('------------------------------------------');
          console.log(resp.data.length);
        }
      });
      
    },

    obtenerReservas1: async function () {
      console.log("Hola uuuu");
      await axios.get("http://localhost:3000/reserva/list2").then((resp) => {
        if (resp.status == 200) {
          this.reservaVisualizar1 = resp.data.length;
          console.log('------------------------------------------');
          console.log(resp.data.length);
        }
      });
      
    },

    obtenerReservas2: async function () {
      console.log("Hola uuuu");
      await axios.get("http://localhost:3000/reserva/list2").then((resp) => {
        if (resp.status == 200) {
          this.reservaVisualizar2 = resp.data.length;
          console.log('------------------------------------------');
          console.log(resp.data.length);
        }
      });
      
    },

  }

};
</script>