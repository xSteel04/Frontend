<template>
  <v-container>
    
    <v-row>
      <v-col md="6" id="text-title">
        <h3>Mantenimiento de habitaciones</h3>
      </v-col>

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
          Crear habitación
        </v-btn>
      </template>
      <v-card>
        <v-card-title>
          <span class="text-h5">Habitación</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12" >
                <label>Imagen</label>
                <v-text-field
                v-model="habitacion.img"
                  label="Link"
                  :rules="Rules"
                  required
                ></v-text-field>
              </v-col>

              <v-col cols="12" >
                <label>Nombre de la habitación</label>
                <v-text-field
                v-model="habitacion.nombre"
                  label="Nombre"
                  :rules="Rules"
                  required
                ></v-text-field>
              </v-col>

              <v-col cols="4">
                <label>Categoría</label>
                <v-select
                v-model="habitacion.idcategoriaHab"
                :items="categoriaHabitacion"
                      item-text="titulo"
                      item-value="idcategoriaHab"
                  :rules="Rules"
                  required
                ></v-select>
              </v-col>

              <v-col cols="4">
                <label>Equipamiento</label>
                <v-select
                v-model="habitacion.idequipamiento"
                :items="equipamientoVisualizar"
                      item-text="idequipamiento"
                      item-value="idequipamiento"
                  :rules="Rules"
                  required
                ></v-select>
              </v-col>

              <v-col cols="4">
                <label>Precio</label>
                <v-text-field
                v-model="habitacion.precio"
                  label="Precio"
                  type="number"
                  :rules="Rules"
                  required
                ></v-text-field>
              </v-col>

              <v-col cols="12">
                <label>Descripción</label>
                <v-text-field
                v-model="habitacion.descripcion"
                  label="Descripcion"
                  type="text"
                  :rules="Rules"
                  required
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
          <v-btn color="blue darken-1" text @click="createHabitacion()">
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
                <th class="text-left">Equipamiento</th>
                <th class="text-left">Categoría</th>
                <th class="text-left">Estado</th>
                <th class="text-left">Precio</th>
                <th class="text-left">Descripción</th>
                <th class="text-left">Fecha de creación</th>
                <th class="text-left"></th>
                <th class="text-left"></th>
              </tr>
            </thead>


            <tbody v-for="(item, i) in habitacionesVisualizar" :key="i">
              <tr>
                <td>
                  <img :src="item.img" width="150" alt="">
                </td>

                <td>{{ item.nombre }}</td>

                <td>
                  <div v-for="(itemEquip, i) in equipamientoVisualizar" :key="i">
                    <ul>
                      <div v-if="item.idequipamiento == itemEquip.idequipamiento">
                        <li>
                          <div v-if="itemEquip.aireacondicionado == true">Aire: Si</div>
                          <div v-if="itemEquip.aireacondicionado == false">Aire: No</div>
                        </li>
                        <li>Baños: {{ itemEquip.baños }}</li>
                        <li>Camas: {{ itemEquip.camas }}</li>
                        <li>
                          <div v-if="itemEquip.internet== true">Internet: Si</div>
                          <div v-if="itemEquip.internet == false">Internet: No</div>
                        </li>
                        <li>Televisores:{{ itemEquip.televisores }}</li>
                      </div>
                    </ul>
                  </div>
                </td>

                <td>
                  <div v-for="(itemCate, i) in categoriaHabitacion" :key="i">
                    <div v-if="item.idcategoriaHab == itemCate.idcategoriaHab">
                      {{ itemCate.titulo }}
                    </div>
                  </div>
                </td>

                <td>{{ item.estado }}</td>
                <td>${{ item.precio }}</td>
                <td>{{ item.descripcion }}</td>
                <td>{{ item.created_at.substr(2, 8) }}</td>
                
                <td>
                   <div class="text-center">
                      <v-dialog  v-model="dialog2"
                          persistent
                          max-width="600px">
                        <template v-slot:activator="{ on, attrs }">
                            <v-btn
                            @click="stateRolsUsers2(item.idhabitacion,)"
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
                                  <label>Nombre de la habitación</label>
                                  <v-text-field
                                  v-model="ObtenerRoles.nombre"
                                    :rules="Rules"
                                    required
                                  ></v-text-field>
                                </v-col>

                                <v-col cols="12" >
                                  <label>Imagen</label>
                                  <v-text-field
                                  v-model="ObtenerRoles.img"
                                    label="Link"
                                    :rules="Rules"
                                    required
                                  ></v-text-field>
                                </v-col>

                                <v-col cols="4">
                                  <label>Categoría</label>
                                  <v-select
                                  v-model="ObtenerRoles.idcategoriaHab"
                                  :items="categoriaHabitacion"
                                        item-text="titulo"
                                        item-value="idcategoriaHab"
                                    :rules="Rules"
                                    required
                                  ></v-select>
                                </v-col>

                                <v-col cols="4">
                                  <label>Equipamiento</label>
                                  <v-select
                                  v-model="ObtenerRoles.idequipamiento"
                                  :items="equipamientoVisualizar"
                                        item-text="idequipamiento"
                                        item-value="idequipamiento"
                                    :rules="Rules"
                                    required
                                  ></v-select>
                                </v-col>

                                <v-col cols="4">
                                  <label>Precio</label>
                                  <v-text-field
                                  v-model="ObtenerRoles.precio"
                                    label="Precio"
                                    type="number"
                                    :rules="Rules"
                                    required
                                  ></v-text-field>
                                </v-col>

                                <v-col cols="12">
                                  <label>Descripción</label>
                                  <v-text-field
                                  v-model="ObtenerRoles.descripcion"
                                    label="Descripcion"
                                    type="text"
                                    :rules="Rules"
                                    required
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
                        <v-btn color="blue darken-1" text @click="cambiarRol(item.idhabitacion,ObtenerRoles)">
                          Crear
                        </v-btn>
                      </v-card-actions>
                    </v-card>
                    </v-dialog>
                    </div>
                </td>

                <td>
                  <v-btn 
                  class="mr-4" 
                   @click="stateHab(item.idhabitaciones, estadoActivado)" 
                    color="waring"
                    v-bind="attrs" 
                    v-on="on" 
                    fab
                    dark
                    small
                    >
                    <v-icon dark>
                      mdi-eye-off
                    </v-icon>
                  </v-btn>
                  
                </td>
              </tr>
            </tbody>
          </template>
        </v-simple-table>
      </v-col>


    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  data: () => ({
      dialog: false,
      dialog2: false,
      dialog3: false,
      obtener: 0,
      estadoActivado: true,
      estadoDesactivado: false,
      
    Servicioshabitacion: [],
    habitacionesVisualizar: [],
    categoriaHabitacion: [],
    equipamientoVisualizar: [], 
   
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
    
     habitacion: {
      nombre: "",
      estado: true,
      descripcion: "",
      img: "",
      precio: 0,
      idequipamiento: 0,
      idcategoriaHab: 0,
      idreserva: 0,
    },

        
     habitacion2: {
      nombre: "",
      estado: true,
      descripcion: "",
      img: "",
      precio: 0,
      idequipamiento: 0,
      idcategoriaHab: 0,
      idreserva: 0,
    },
     Obtener:{
       nombre: "",
      estado: true,
      descripcion: "",
      img: "",
      precio: 0,
      idequipamiento: 0,
      idcategoriaHab: 0,
      idreserva: 0,
    },
    idrol:0,
     ObtenerRoles:[],
    
   
  }),

  created() {
    this.obtenerHabitaciones();
     this.obtenerReservas();
     this.obtenerEquipamiento();
     this.categoriaHabitacion();
     
  },

  methods: {
 stateRolsUsers2: async function (id) {
      this.idrol = parseInt(id);
      console.log("Id rol: ", this.idrol);
      this.obtenerRoles();     
    },
    obtenerRoles: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/habitaciones/obtener").then((resp) => {
        if (resp.status == 200) {
          for (let index = 0; index < resp.data.length; index++) {
            const element = resp.data[index];
            if (element.idhabitacion == this.idrol) {
              this.ObtenerRoles = element;             
            }
          }
          console.log("Yo soy el rol: ", this.ObtenerRoles);
        }
      });
    },
    cambiarRol: async function (id) {
      let Aidi = parseInt(id);
      let estado = this.ObtenerRoles;
      console.log("Id Reserva: ", Aidi, "Estado: ", estado);
      await axios
        .put("http://localhost:3000/habitaciones/update/" + this.idrol, estado)
        .then((resp) => {
          if (resp.status == 204) {
            location.reload();
          }
        });
    },
   

    stateHab: async function (id, estadoCambio) {
          let Aidi = parseInt(id)
          let estado = {estado: estadoCambio};
          console.log("Id Habitacion: ", Aidi, "Estado: ", estado);
        await axios.put("http://localhost:3000/habitaciones/delete/"+Aidi, estado).then((resp) => {
            if (resp.status == 204) {
              location.reload();
            }
        });
      },
     obtenerEquipamiento: async function () {
      await axios.get("http://localhost:3000/equipamiento").then((resp) => {
        if (resp.status == 200) {
          this.equipamientoVisualizar = resp.data;
        }
      });
    },
    obtenerServicios: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/servicios").then((resp) => {
        if (resp.status == 200) {
          this.Servicioshabitacion = resp.data;
        }
      });
      console.log(this.Servicioshabitacion);
    },
    obtenerTipoHabitaciones: async function () {
      await axios.get("http://localhost:3000/categoriaHab").then((resp) => {
        if (resp.status == 200) {
          this.categoriaHabitacion = resp.data;
        }
      });
    },
    obtenerHabitaciones: async function () {
      await axios.get("http://localhost:3000/habitaciones/obtener").then((resp) => {
        if (resp.status == 200) {
          this.habitacionesVisualizar = resp.data;
          this.obtenerTipoHabitaciones()
          this.obtenerEquipamiento();
        }
      });
    },
    obtenerEquipamiento: async function () {
      await axios.get("http://localhost:3000/equipamiento").then((resp) => {
        if (resp.status == 200) {
          this.equipamientoVisualizar = resp.data;
        }
      });
    },
    createHabitacion: async function (){
      await axios
        .post("http://localhost:3000/habitaciones/create", this.habitacion)
        .then((resp) => {
          if (resp.status == 201) {
            alert("La habitación fue creada exitosamente.");
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
  updateHabitacion: async function (){
      await axios
        .post("http://localhost:3000/habitaciones/update" + this.obtener + "/", this.habitacion)
        .then((resp) => {
          if (resp.status == 204) {
            alert("La habitación fue actualizada exitosamente.");
            location.reload();
          }
    })
      .catch(error => {
        let respu = error.message
        alert(respu);
        console.log(this.respuesta);
      })
    },
};

</script>


<style scoped>
#text-title {
  margin-top: 20px;
  margin-left: 40%;
}
</style>