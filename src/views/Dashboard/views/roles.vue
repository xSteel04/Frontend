<template>
  <v-container>
    <v-row>
      <v-col md="6" id="text-title">
        <h3>Roles</h3>
      </v-col>
    </v-row>

      <v-col justify="start">
        <v-dialog v-model="dialogCrear" persistent max-width="600px">
          <template v-slot:activator="{ on, attrs }">
            <v-btn color="primary" dark v-bind="attrs" v-on="on">
              Asignar
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">Asignar roles de usuario</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-form v-model="validacionCreacion">
                  <v-row no-gutters>
                    <v-col cols="12" sm="6">
                      <v-select
                        v-model="create.idUsuario"
                        :items="usuarios"
                        item-text="nombre"
                        item-value="idUsuario"
                        label="Usuario"
                        :rules="Rules"
                        required
                      ></v-select>
                    </v-col>
                    <v-col cols="12" sm="6">
                      <v-select
                        v-model="create.idRol"
                        :items="rol"
                        item-text="nombreRol"
                        item-value="idrRol"
                        label="Roles"
                        :rules="Rules"
                        required
                      ></v-select>
                    </v-col>

                    <!-- <v-col cols="12" sm="6">
                      <v-text-field
                        type="number"
                        label="Cantidad de personas"
                        :rules="Rules"
                        required
                      ></v-text-field>
                    </v-col> -->

                    <v-col md="3" style="margin-right: 390px">
                      <v-btn
                        @click="createRolsUsers()"
                        :disabled="!validacionCreacion"
                        class="mr-4"
                        color="success"
                      >
                        Enviar
                      </v-btn>
                    </v-col>
                  </v-row>
                </v-form>
              </v-container>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="dialogCrear = false">
                Cerrar
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-col>

      <v-col justify="end">
        <v-dialog v-model="dialogCrear2" persistent max-width="600px">
          <template v-slot:activator="{ on, attrs }">
            <v-btn color="primary" dark v-bind="attrs" v-on="on">
              Crear
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">Crear nuevo rol</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-form v-model="validacionCreacion2">
                  <v-row no-gutters>
                    <v-col cols="12" sm="6">
                      <label>Nombre</label>
                      <v-text-field
                        v-model="create2.nombreRol"
                        :rules="Rules"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6">
                      <v-select
                        v-model="create2.estado"
                        :items="boolean"
                        item-text="label"
                        item-value="condicional"
                        label="estado"
                        :rules="Rules"
                        required
                      ></v-select>
                    </v-col>

                    <v-col cols="12" sm="6">
                      <v-select
                        v-model="create2.reservas"
                        :items="boolean"
                        item-text="label"
                        item-value="condicional"
                        label="reservas"
                        :rules="Rules"
                        required
                      ></v-select>
                    </v-col>

                    <v-col cols="12" sm="6">
                      <v-select
                        v-model="create2.check_in"
                        :items="boolean"
                        item-text="label"
                        item-value="condicional"
                        label="check_in"
                        :rules="Rules"
                        required
                      ></v-select>
                    </v-col>

                    <v-col cols="12" sm="6">
                      <v-select
                        v-model="create2.check_out"
                        :items="boolean"
                        item-text="label"
                        item-value="condicional"
                        label="check_out"
                        :rules="Rules"
                        required
                      ></v-select>
                    </v-col>

                    <v-col cols="12" sm="6">
                      <v-select
                        v-model="create2.habitaciones"
                        :items="boolean"
                        item-text="label"
                        item-value="condicional"
                        label="habitaciones"
                        :rules="Rules"
                        required
                      ></v-select>
                    </v-col>

                    <v-col cols="12" sm="6">
                      <v-select
                        v-model="create2.usuarios"
                        :items="boolean"
                        item-text="label"
                        item-value="condicional"
                        label="usuarios"
                        :rules="Rules"
                        required
                      ></v-select>
                    </v-col>
                    <v-col cols="12" sm="6">
                      <v-select
                        v-model="create2.datos"
                        :items="boolean"
                        item-text="label"
                        item-value="condicional"
                        label="usuarios"
                        :rules="Rules"
                        required
                      ></v-select>
                    </v-col>
                    <v-col cols="12" sm="6">
                      <v-select
                        v-model="create2.cuentas"
                        :items="boolean"
                        item-text="label"
                        item-value="condicional"
                        label="usuarios"
                        :rules="Rules"
                        required
                      ></v-select>
                    </v-col>
                    <v-col cols="12" sm="6">
                      <v-select
                        v-model="create2.aprobarclientes"
                        :items="boolean"
                        item-text="label"
                        item-value="condicional"
                        label="usuarios"
                        :rules="Rules"
                        required
                      ></v-select>
                    </v-col>
                    <v-col cols="12" sm="6">
                      <v-select
                        v-model="create2.listaclientes"
                        :items="boolean"
                        item-text="label"
                        item-value="condicional"
                        label="usuarios"
                        :rules="Rules"
                        required
                      ></v-select>
                    </v-col>
                    <v-col cols="12" sm="6">
                      <v-select
                        v-model="create2.editarhabitacion"
                        :items="boolean"
                        item-text="label"
                        item-value="condicional"
                        label="usuarios"
                        :rules="Rules"
                        required
                      ></v-select>
                    </v-col>
                    <v-col md="3" style="margin-right: 390px">
                      <v-btn
                        @click="createRol()"
                        :disabled="!validacionCreacion2"
                        class="mr-4"
                        color="success"
                      >
                        Enviar
                      </v-btn>
                    </v-col>
                  </v-row>
                </v-form>
              </v-container>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="dialogCrear2 = false">
                Cerrar
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-col>
      

      <v-col md="12">
        <v-simple-table height="600px">
          <template v-slot:default>
            <thead>
              <tr>
                <th class="text-left">Nombre de usuario</th>
                <th class="text-left">Rol</th>
                <th class="text-left">Estado</th>
                <th class="text-left">Cliente</th>
                <th class="text-left">Trabajador</th>
                <th class="text-left">Estado</th>
                <th class="text-left">Eliminar rol de usuario</th>
                <th class="text-left">Modificar rol</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, i) in ObtenerUsuarios" :key="i">
                <td>
                  <div v-for="(itemUsuario, i) in usuarios" :key="i">
                    <div v-if="itemUsuario.idUsuario == item.idUsuario">
                      {{ itemUsuario.nombre }}
                    </div>
                  </div>
                </td>
                <td>
                  <div v-for="(itemRol, i) in rol" :key="i">
                    <div v-if="itemRol.idrRol == item.idRol">
                      {{ itemRol.nombreRol }}
                    </div>
                  </div>
                </td>
                <td>
                  <div v-if="item.estado == true">Activo</div>
                  <div v-if="item.estado == false">Inactivo</div>
                </td>
                <td>
                  <div v-if="item.cliente == true">Si</div>
                  <div v-if="item.cliente == false">No</div>
                </td>
                <td>
                  <div v-if="item.trabajador == true">Si</div>
                  <div v-if="item.trabajador == false">No</div>
                </td>
                <td>
                  <v-btn
                    class="mx-2"
                    v-if="item.estado == true"
                    @click="stateRolsUsers(item.idUsuarioRol, estadoDesactivado)"
                    fab
                    dark
                    small
                    color="red"
                  >
                    <v-icon dark> mdi-account-convert </v-icon>
                  </v-btn>

                  <v-btn
                    class="mx-2"
                    v-if="item.estado == false"
                    @click="stateRolsUsers(item.idUsuarioRol, estadoActivado)"
                    fab
                    dark
                    small
                    color="green"
                  >
                    <v-icon dark> mdi-account-convert </v-icon>
                  </v-btn>
                </td>
                <td>
                <v-btn
                    class="mx-10"
                    @click="deleteRol(item.idUsuarioRol)"
                    fab
                    dark
                    small
                    color="red"
                  >
                    <v-icon red> mdi-delete </v-icon>
                  </v-btn>
                </td>
                <td>
                  <v-row justify="start">
                    <v-dialog v-model="dialogCrear3" max-width="600px">
                      <template v-slot:activator="{ on, attrs }">
                        <v-btn
                          @click="stateRolsUsers2(item.idRol,)"
                          color="gray"
                          dark
                          v-bind="attrs"
                          v-on="on"
                          circle
                        >
                          <v-icon dark> mdi-pen </v-icon>
                        </v-btn>
                      </template>
                      <v-card>
                        <v-card-title>
                          <span class="text-h5">Editar permisos</span>
                        </v-card-title>
                        <v-card-text>
                          <v-container>
                            <v-form v-model="validacionCreacion3">
                              <v-row no-gutters>
                                <v-col cols="12" sm="6">
                                  <label>Nombre</label>
                                  <v-text-field
                                    v-model="ObtenerRoles.nombreRol"
                                    item-value="nombreRol"
                                  ></v-text-field>
                                </v-col>
                               

                                <v-col cols="12" sm="6">
                                  <v-select
                                    v-model="ObtenerRoles.reservas"
                                    :items="boolean"
                                    item-text="label"
                                    item-value="condicional"
                                    label="reservas"
                                  ></v-select>
                                </v-col>

                                <v-col cols="12" sm="6">
                                  <v-select
                                    v-model="ObtenerRoles.check_in"
                                     :items="boolean"
                                    item-text="label"
                                    item-value="condicional"
                                    label="check_in"
                                  ></v-select>
                                </v-col>

                                <v-col cols="12" sm="6">
                                  <v-select
                                    v-model="ObtenerRoles.check_out"
                                    :items="boolean"
                                    item-text="label"
                                    item-value="condicional"
                                    label="check_out"
                                  ></v-select>
                                </v-col>

                                <v-col cols="12" sm="6">
                                  <v-select
                                    v-model="ObtenerRoles.habitaciones"
                                    :items="boolean"
                                    item-text="label"
                                    item-value="condicional"
                                    label="habitaciones"
                                  ></v-select>
                                </v-col>

                                <v-col cols="12" sm="6">
                                  <v-select
                                    v-model="ObtenerRoles.usuarios"
                                    :items="boolean"
                                    item-text="label"
                                    item-value="condicional"
                                    label="usuarios"
                                  ></v-select>
                                </v-col>
                                <v-col cols="12" sm="6">
                                  <v-select
                                    v-model="ObtenerRoles.datos"
                                    :items="boolean"
                                    item-text="label"
                                    item-value="condicional"
                                    label="datos"
                                  ></v-select>
                                </v-col>
                                <v-col cols="12" sm="6">
                                  <v-select
                                    v-model="ObtenerRoles.cuentas"
                                    :items="boolean"
                                    item-text="label"
                                    item-value="condicional"
                                    label="cuentas"
                                  ></v-select>
                                </v-col>
                                <v-col cols="12" sm="6">
                                  <v-select
                                    v-model="ObtenerRoles.aprobarclientes"
                                    :items="boolean"
                                    item-text="label"
                                    item-value="condicional"
                                    label="aprobarclientes"
                                  ></v-select>
                                </v-col>
                                <v-col cols="12" sm="6">
                                  <v-select
                                    v-model="ObtenerRoles.listaclientes"
                                    :items="boolean"
                                    item-text="label"
                                    item-value="condicional"
                                    label="listaclientes"
                                  ></v-select>
                                </v-col>
                                <v-col cols="12" sm="6">
                                  <v-select
                                    v-model="ObtenerRoles.editarhabitacion"
                                    :items="boolean"
                                    item-text="label"
                                    item-value="condicional"
                                    label="editarhabitacion"
                                  ></v-select>
                                </v-col>

                                <v-col md="3" style="margin-right: 390px">
                                  <v-btn
                                    @click="cambiarRol(item.idUsuarioRol,ObtenerRoles)"
                                    :disabled="!validacionCreacion3"
                                    class="mr-4"
                                    color="success"
                                  >
                                    Enviar
                                  </v-btn>
                                </v-col>
                              </v-row>
                            </v-form>
                          </v-container>
                        </v-card-text>
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
    
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  data: () => ({
    ObtenerUsuarios: [],
    ObtenerEntidades: [],
    usuarios: [],
    rol: [],
    dialogCrear: false,
    validacionCreacion: true,
    dialogCrear2: false,
    validacionCreacion2: true,
    dialogCrear3: false,
    validacionCreacion3: true,
    menu: false,
    menu2: false,
    Rules: [(v) => !!v || "El campo es requerido"],

    estadoActivado: true,
    estadoDesactivado: false,

    boolean: [
      {
        cliente: "No es cliente",
        label: "visualizar",
        condicional: 1,
      },
      {
        cliente: "Es cliente",
        label: "No visualizar",
        condicional: 0,
      },
    ],
    create: {
      idUsuario: 0,
      idRol: 0,
      cliente: false,
      trabajador: true,
      estado: false,
    },
    create2: {
      nombreRol: 0,
      estado: 1,
      reservas: 0,
      check_in: 0,
      check_out: 0,
      habitaciones: 0,
      usuarios: 0,
      datos: 0,
      cuentas: 0,
      aprobarclientes: 0,
      listaclientes: 0,
      editarhabitacion: 0,
      vista1: 0,
      vista2: 0,
      vista3: 0,
      vista4: 0,
      vista5: 0,
    },
    create3: {
      nombreRol: 0,
      estado: 1,
      reservas: 0,
      check_in: 0,
      check_out: 0,
      habitaciones: 0,
      usuarios: 0,
      datos: 0,
      cuentas: 0,
      aprobarclientes: 0,
      listaclientes: 0,
      editarhabitacion: 0,
      vista1: 0,
      vista2: 0,
      vista3: 0,
      vista4: 0,
      vista5: 0,
    },
    idrol: 0,
    ObtenerRoles: {
      nombreRol: 0,
      estado: 1,
      reservas: 0,
      check_in: 0,
      check_out: 0,
      habitaciones: 0,
      usuarios: 0,
      datos: 0,
      cuentas: 0,
      aprobarclientes: 0,
      listaclientes: 0,
      editarhabitacion: 0,
      vista1: 0,
      vista2: 0,
      vista3: 0,
      vista4: 0,
      vista5: 0,
    },
  }),

  created() {
    this.obtenerUsuariosRoles();
    this.obtenerUsuarios();
    this.obtenerRol();
  },

  methods: {

    deleteRol :async function (id)  {
       let Aidi = parseInt(id);
      console.log("Hola", Aidi);
  await axios
    .delete("http://localhost:3000/usuariorol/delete/" + Aidi)
    .then((resp) => {
        if (resp.status == 500){
          this.idRol = resp.data;
          console.log("borrar", this.idRol);
        }
         location.reload();
    });
},

    stateRolsUsers: async function (id, estadoCambio) {
      let Aidi = parseInt(id);
      let estado = { estado: estadoCambio };
      console.log("Id Reserva: ", Aidi, "Estado: ", estado);
      await axios
        .put("http://localhost:3000/usuariorol/cambioEstado/" + Aidi, estado)
        .then((resp) => {
          if (resp.status == 204) {
            location.reload();
          }
        });
    },

    stateRolsUsers2: async function (id) {
      this.idrol = parseInt(id);
      console.log("Id rol: ", this.idrol);
      this.obtenerRoles();     
    },
    obtenerRoles: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/rol").then((resp) => {
        if (resp.status == 200) {
          for (let index = 0; index < resp.data.length; index++) {
            const element = resp.data[index];
            if (element.idrRol == this.idrol) {
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
        .put("http://localhost:3000/rol/update/" + this.idrol, estado)
        .then((resp) => {
          if (resp.status == 204) {
            location.reload();
          }
        });
    },
    createRolsUsers: async function () {
      if (this.create.estado == 1) {
        this.create.estado = false;
      }
      if (this.create.estado == 2) {
        this.create.estado = true;
      }
      await axios
        .post("http://localhost:3000/usuariorol/create", this.create)
        .then((resp) => {
          if (resp.status == 201) {
            alert("EL rol del usuariario fue asignado exitosamente.");
            location.reload();
          }
        });
    },
    createRol: async function () {
      await axios.post("http://localhost:3000/rol/create", this.create2).then((resp) => {
        if (resp.status == 201) {
          alert("EL rol del usuariario fue asignado exitosamente.");
          location.reload();
        }
      });
    },

    obtenerUsuarios: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/usuario/list1").then((resp) => {
        if (resp.status == 200) {
          this.usuarios = resp.data;
        }
      });
    },

    obtenerRol: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/rol").then((resp) => {
        if (resp.status == 200) {
          this.rol = resp.data;
        }
      });
    },

    obtenerUsuariosRoles: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/usuariorol").then((resp) => {
        if (resp.status == 200) {
          this.ObtenerUsuarios = resp.data;
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
