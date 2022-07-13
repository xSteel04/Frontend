<template>
  <v-container>
    <v-row>
      <v-col md="3">
        <label for="BusquedaEstado">Seleccione un estado</label>
        <v-row justify="center" style="margin-top: 5px">
          <v-col md="12">
            <v-select
              :items="estado"
              item-text="nombre"
              item-value="nombre"
              menu-props="auto"
              v-model="buscar"
              label="Estados"
            >
            </v-select>
          </v-col>
        </v-row>
      </v-col>
      <v-col md="6" id="text-title">
        <h3>Historial de habitaciones</h3>
      </v-col>
      <v-col md="12">
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

                <th class="text-left">
                  <v-select
                    :items="estado"
                    item-text="nombre"
                    item-value="nombre"
                    menu-props="auto"
                    v-model="EstadoSeleccionado"
                    label="Estados"
                  ></v-select>
                </th>
              </tr>
            </thead>
            <tbody v-for="(item, i) in habitacionesVisualizar" :key="i">
              <tr v-if="item.estado == buscar">
                <td>
                  <img :src="item.img" width="150" alt="" />
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
                        <li>Baños: {{ itemEquip.internet }}</li>
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
                  <v-btn
                    depressed
                    @click="CambioEstado(item.idhabitacion, EstadoSeleccionado)"
                    color="yellow"
                  >
                    Cambiar
                  </v-btn>
                </td>
                <td>
                  <div class="text-center">
                    <v-dialog v-model="dialog" width="600">
                      <template v-slot:activator="{ on, attrs }">
                        <v-btn
                          v-if="item.estado == 1"
                          class="mr-4"
                          color="success"
                          v-bind="attrs"
                          v-on="on"
                          fab
                          dark
                          small
                        >
                          <v-icon dark> mdi-account-minus </v-icon>
                        </v-btn>
                      </template>

                      <v-card>
                        <v-card-title class="text-h5 blue-grey darken-2">
                          Condiciones para la reservación 
                        </v-card-title>
                        <v-card-text>
                          Al hacer click en el boton de abajo acepto que revise las políticas, términos y condiciones
                          , las alertas de viajes del gobierno y las normas y restricciones internacionales del turismo.
                          <v-checkbox
                            style="margin-top: 20px"
                            v-model="checkbox1"
                            label="Acepto las condiciones de uso"
                          ></v-checkbox>
                        </v-card-text>

                        <v-divider></v-divider>

                        <v-card-actions>
                          <v-spacer></v-spacer>
                          <v-btn
                            color="primary"
                            text
                            :disabled="!checkbox1"
                            @click="EliminarReservas(item.idreserva), (dialog = false)"
                          >
                            Cancelar
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
      <v-col v-if="buscar == 'Ocupada'" md="12">
        <v-simple-table height="300px">
          <template v-slot:default>
            <thead>
              <tr>
                <th class="text-left">Usuario</th>
                <th class="text-left">Tipo habitación</th>
                <th class="text-left">Habitación</th>
                <th class="text-left">Servicio</th>
                <th class="text-left">Catidad de personas</th>
                <th class="text-left">Fecha de inicio</th>
                <th class="text-left">Fecha de salida</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, i) in reservaVisualizar" :key="i">
                <td>
                  <div v-for="(itemUser, i) in usuariosVisualizar" :key="i">
                    <div v-if="item.idusuario == itemUser.idUsuario">
                      {{ itemUser.nombre }}
                    </div>
                  </div>
                </td>
                <td>
                  <div v-for="(itemCate, i) in categoriaHabitacion" :key="i">
                    <div v-if="item.idcategoriaHab == itemCate.idcategoriaHab">
                      {{ itemCate.titulo }}
                    </div>
                  </div>
                </td>
                <td>
                  <div v-for="(itemHab, i) in habitacionesVisualizar" :key="i">
                    <div v-if="item.idhabitacion == itemHab.idhabitacion">
                      {{ itemHab.nombre }}
                    </div>
                  </div>
                </td>
                <td>
                  <div v-for="(itemServ, i) in Servicioshabitacion" :key="i">
                    <div v-if="item.idservicio == itemServ.idservicio">
                      {{ itemServ.nombre }}
                    </div>
                  </div>
                </td>
                <th>{{ item.cantPersonas }}</th>
                <td>{{ item.fechaEntrada }}</td>
                <td>{{ item.fechaSalida }}</td>
                <td>
                  <v-row justify="start">
                    <v-dialog v-model="dialogCrear3" max-width="600px">
                      <template v-slot:activator="{ on, attrs }">
                        <v-btn
                          v-bind="attrs"
                          v-on="on"
                          class="mx-10"
                          @click="VizualizarAcompa(item.idreserva)"
                          fab
                          dark
                          small
                          color="blue"
                        >
                          <v-icon red> mdi-account-multiple </v-icon>
                        </v-btn>
                      </template>
                      <v-card>
                        <v-simple-table>
                          <thead>
                            <tr>
                              <th class="text-left">Nombre</th>
                              <th class="text-left">Edad</th>
                              <th class="text-left">Identificacion</th>
                              <th class="text-left">Telefono</th>
                              <th class="text-left">Reserva</th>
                            </tr>
                          </thead>
                          <tbody>
                            <tr v-for="(item, i) in vizualizaracompanante" :key="i">
                              <td>{{ item.Nombre }}</td>
                              <td>{{ item.Edad }}</td>
                              <td>{{ item.Identificacion }}</td>
                              <td>{{ item.Telefono }}</td>
                            </tr>
                          </tbody>
                        </v-simple-table>
                        <v-card-actions>
                          <v-spacer></v-spacer>
                          <v-btn color="blue darken-1" text @click="dialogCrear3 = false">
                            Cerrar
                          </v-btn>
                        </v-card-actions>
                      </v-card>
                    </v-dialog>
                  </v-row>
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
    contador: 0,
    reservaVisualizar: [],
    Servicioshabitacion: [],
    usuariosVisualizar: [],
    ObtenerUsuarios: [],
    habitacionesVisualizar: [],
    categoriaHabitacion: [],
    equipamientoVisualizar: [],
    EstadoSeleccionado: "",
    buscar: "",
    estado: [
      {
        nombre: "Reservadas",
      },
      {
        nombre: "Limpieza",
      },
      {
        nombre: "Ocupada",
      },
      {
        nombre: "Disponible",
      },
    ],
    reservar: {
      fechaEntrada: "",
      fechaSalida: "",
      cantPersonas: 0,
      estado: true,
      idservicio: 0,
      idhabitacion: 0,
      idcategoriaHab: 0,
      idusuario: 0,
    },
    vizualizaracompanante:[]
  }),

  created() {
    this.obtenerHabitaciones();
    this.obtenerReservas();
  },

  methods: {
    VizualizarAcompa: async function (id) {
      let reserva = parseInt(id)
      console.log("Hola", reserva);
      await axios.get("http://localhost:3000/acompanante/list2/" + reserva).then((resp) => {
        console.log("mama",resp)
        if (resp.status == 200) {
          this.vizualizaracompanante = resp.data;
          console.log("Hola", this.vizualizaracompanante);
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
    obtenerReservas: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/reserva").then((resp) => {
        if (resp.status == 200) {
          this.reservaVisualizar = resp.data;
          this.obtenerUsuarios();
          this.obtenerTipoHabitaciones();
          this.obtenerHabitaciones();
          this.obtenerServicios();
        }
      });
      console.log(this.reservaVisualizar);
    },
    obtenerUsuarios: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/usuario").then((resp) => {
        if (resp.status == 200) {
          this.usuariosVisualizar = resp.data;
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

    CambioEstado: async function (id, estadoCambio) {
      let Aidi = parseInt(id);
      let estado = { estado: estadoCambio };
      if (this.EstadoSeleccionado == "") {
        alert("Selecciona un estado");
        return 0;
      }
      await axios
        .put("http://localhost:3000/habitaciones/cambiarEstado/" + Aidi, estado)
        .then((resp) => {
          if (resp.status == 204) {
            this.obtenerHabitaciones();
          }
        });
    },
    obtenerTipoHabitaciones: async function () {
      await axios.get("http://localhost:3000/categoriaHab").then((resp) => {
        if (resp.status == 200) {
          this.categoriaHabitacion = resp.data;
        }
      });
    },
    Contador: async function () {
      this.contador++;
      console.log("contador", this.contador);
    },
    obtenerHabitaciones: async function () {
      await axios.get("http://localhost:3000/habitaciones/obtener").then((resp) => {
        if (resp.status == 200) {
          this.habitacionesVisualizar = resp.data;

          this.obtenerTipoHabitaciones();
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
  },
};
</script>

<style scoped>
#text-title {
  margin-top: 20px;
  margin-left: 40%;
}
</style>
