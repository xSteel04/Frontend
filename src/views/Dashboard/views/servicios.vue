<template>
  <v-container>
    <v-row>
      <v-col  id="text-title">
        <center><h3>Servicios</h3></center>
      </v-col>
    </v-row>

    <v-row justify="start">
    <v-dialog
      v-model="dialog"
      persistent
      max-width="600px"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="primary"
          dark
          v-bind="attrs"
          v-on="on"
        >
          Crear servicio
        </v-btn>
      </template>
      <v-card>
        <v-card-title>
          <span class="text-h5">Servicio</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12" >
                <label>Imagen</label>
                <v-text-field
                 v-model="servicios.img"
                ></v-text-field>
              </v-col>
              
              <v-col cols="12" >
                <label>Servicio</label>
                <v-text-field
                v-model="servicios.servicio"
               
                ></v-text-field>
              </v-col>
               <v-col cols="12" >
                                  <v-select
                                    v-model="servicios.estado"
                                    :items="boolean"
                                    item-text="label"
                                    item-value="condicional"
                                    label="Tipo"
                                  ></v-select>
                                </v-col>

            
              <v-col cols="4">
                <label>Precio</label>
                <v-text-field
                v-model="servicios.precio"
                ></v-text-field>
              </v-col>

              <v-col cols="12">
                <label>Descripción</label>
                <v-text-field
                v-model="servicios.descripcion"
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="dialog = false">
            Cerrar
          </v-btn>
          <v-btn color="blue darken-1" text @click="createServicio(servicios)">
            Crear
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>

  <v-col md="12" style="margin-top: 20px;">
        <v-simple-table height="600px">
          <template v-slot:default>

            <thead>
              <tr>
                <th class="text-left">Imagen</th>
                <th class="text-left">Nombre</th>
                <th class="text-left">Precio</th>
                <th class="text-left">Descripción</th>
                <th class="text-left"></th>
              </tr>
            </thead>


            <tbody v-for="(item, i) in servicioVisualizar" :key="i">
              <tr>
                <td>
                  <img :src="item.img" width="150" alt="">
                </td>

                <td>{{ item.servicio }}</td>
                <td>${{ item.precio }}</td>
                <td>{{ item.descripcion }}</td>

                <td>
                   <div class="text-center">
                      <v-dialog  v-model="dialog2"
                          persistent
                          max-width="600px">
                        <template v-slot:activator="{ on, attrs }">
                            <v-btn
                             @click="stateRolsUsers2(item.idservicio,)"
                              rounded
                              dark
                              v-bind="attrs"
                              v-on="on"
                            >
                            <v-icon dark>
                              mdi-pencil
                          </v-icon>
                          </v-btn>
                        </template>

                        <v-card>
                          <v-card-title class="text-h5 blue-grey darken-2">
                            Editar habitación
                          </v-card-title>
                          <v-card-text>
                            <v-container>
                              <v-row>
                                <v-col cols="12" >
                <label>Imagen</label>
                <v-text-field
                 v-model="servicios.img"
                ></v-text-field>
              </v-col>

              <v-col cols="12" >
                <label>Servicio</label>
                <v-text-field
                v-model="servicios.servicio"
               
                ></v-text-field>
              </v-col>
               <v-col cols="12" sm="6">
                                  <v-select
                                    v-model="servicios.estado"
                                    :items="boolean"
                                    item-text="label"
                                    item-value="condicional"
                                    label="datos"
                                  ></v-select>
                                </v-col>

              <v-col cols="4">
                <label>Precio</label>
                <v-text-field
                v-model="servicios.precio"
                ></v-text-field>
              </v-col>

              <v-col cols="12">
                <label>Descripción</label>
                <v-text-field
                v-model="servicios.descripcion"
                ></v-text-field>
              </v-col>
                              </v-row>
                            </v-container>
                      </v-card-text>
                      <v-card-actions>
                        <v-spacer></v-spacer>
                        <v-btn color="blue darken-1" text @click="dialog2 = false">
                          Cerrar
                        </v-btn>
                        <v-btn color="blue darken-1" text @click="cambiarRol(item.idservicio ,servicios)">
                          Crear
                        </v-btn>
                      </v-card-actions>
                    </v-card>
                    </v-dialog>
                    </div>
                </td>
              </tr>
            </tbody>

          </template>
        </v-simple-table>
      </v-col>

  </v-container>
</template>

<script>
import axios from "axios";
export default {
  data: () => ({
    dialog: false,
    dialog2: false,
    obtener: 0,  

    servicioVisualizar: [],
    servicioshabitacion: [],

    servicios: {
      servicio: "",
      precio: 0,
      img: "",
      descripcion: "",
      estado: 1,
    },
     boolean: [
      {
        cliente: "No es cliente",
        label: "servicio",
        condicional: 1,
      },
      {
        cliente: "Es cliente",
        label: "paquete",
        condicional: 0,
      },
    ],

    idrol: 0,
  }),

  created() {
    this.obtenerServicio();
    this.servicio();
    
  },
 
  methods: {
     stateRolsUsers2: async function (id) {
      this.idrol = parseInt(id);
      console.log("Id rol: ", this.idrol);
      this.obtenerRoles();     
    },
     obtenerRoles: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/servicios").then((resp) => {
        if (resp.status == 200) {
          for (let index = 0; index < resp.data.length; index++) {
            const element = resp.data[index];
            if (element.idservicio == this.idrol) {
              this.servicios = element;             
            }
          }
          console.log("Yo soy el rol: ", this.servicios);
        }
      });
    },
    cambiarRol: async function (id) {
      let Aidi = parseInt(id);
      let estado = this.servicios;
      console.log("Id Reserva: ", Aidi, "Estado: ", estado);
      await axios
        .put("http://localhost:3000/servicios/update/" + this.idrol, estado)
        .then((resp) => {
          if (resp.status == 204) {
            location.reload();
          }
        });
    },
    
    obtenerServicio: async function () {
      await axios.get("http://localhost:3000/servicios").then((resp) => {
        if (resp.status == 200) {
          this.servicioVisualizar = resp.data;
        }
      });
    },

    createServicio: async function (){
      let crear = this.servicios;
      console.log("servicio",this.servicios)
      await axios
        .post("http://localhost:3000/servicios/create", crear)
        .then((resp) => {
          if (resp.status == 201) {
            alert("El servicio fue creado exitosamente.");
            location.reload();
          }
    })
      .catch(error => {
        let respu = error.message
        alert(respu);
        console.log(this.respuesta);
      })
    },

  },

  updateServicio: async function (){
      await axios
        .post("http://localhost:3000/servicios/update" + this.obtener + "/", this.servicio)
        .then((resp) => {
          if (resp.status == 204) {
            alert("El servicio fue actualizado exitosamente.");
            location.reload();
          }
    })
      .catch(error => {
        let respu = error.message
        alert(respu);
        console.log(this.respuesta);
      })
    },

  }
</script>
          
      

  

