<template>
  <v-container>
    <v-row>
        <v-col md="6" id="text-title"><h3>Usuarios adminstrativos</h3></v-col>
      <v-col md="12">
          <v-simple-table height="600px">
        <template v-slot:default>
          <thead>
            <tr>
              <th class="text-left">Nombre</th>
              <th class="text-left">Email</th>
              <th class="text-left">Entidad</th>
              <th class="text-left">Estado</th>
              <th class="text-left"></th>
              <th></th>
            </tr>
          </thead>
          <tbody v-for="(item, i) in ObtenerUsuarios" :key="i">
            <tr v-if="item.trabajador == 1">
              <td>{{item.nombre}}</td>
              <td>{{item.email}}</td>
              <td >
                <div v-for="(itemEntid, i) in ObtenerEntidades" :key="i">
                  <div v-if="item.idEntidad == itemEntid.idEntidad">
                    {{itemEntid.primerNombre}} {{itemEntid.primerApellido}}
                  </div>
                </div>
              </td>
              <td>
                <div v-if="item.estado == true">Activo</div>
                <div v-if="item.estado == false">Inactivo</div>
              </td>
              

              <td>
                  <v-btn
                        class="mx-2"
                        v-if="item.estado == true"
                        @click="stateRolsUsers(item.idUsuario, estadoDesactivado)"
                        fab
                        dark
                        small
                        color="red"
                        >
                        <v-icon dark>
                            mdi-account convert
                        </v-icon>
                    </v-btn>

                    <v-btn
                        class="mx-2"
                        v-if="item.estado == false"
                        @click="stateRolsUsers(item.idUsuario, estadoActivado)"
                        fab
                        dark
                        small
                        color="yellow"
                        >
                        <v-icon dark>
                            mdi-account-convert
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
      ObtenerUsuarios: [],
      ObtenerEntidades: [],
      estadoActivado: true,
      estadoDesactivado: false,
  }),

  created(){
      this.obtenerUsuarios();
  },

  methods:{

      stateRolsUsers: async function (id, estadoCambio) {
          let Aidi = parseInt(id)
          let estado = {estado: estadoCambio};
          console.log("Id Reserva: ", Aidi, "Estado: ", estado);
        await axios.put("http://localhost:3000/usuario/delete/"+Aidi, estado).then((resp) => {
            if (resp.status == 204) {
              location.reload();
            }
        });
      },

      obtenerUsuarios: async function () {
        console.log("Hola");
        await axios.get("http://localhost:3000/usuario").then((resp) => {
            if (resp.status == 200) {
            this.ObtenerUsuarios = resp.data;
            this.obtenerEntidad()
            }
        });
        console.log(this.ObtenerUsuarios);
      },

      obtenerEntidad: async function () {
        console.log("Hola");
        await axios.get("http://localhost:3000/entidad").then((resp) => {
            if (resp.status == 200) {
            this.ObtenerEntidades = resp.data;
            }
        });
        console.log(this.ObtenerEntidades);
      },
      BorrarUsuarios: async function (id) {
        console.log("Hola");
        await axios.delete("http://localhost:3000/usuario/delete/"+id).then((resp) => {
            if (resp.status == 204) {
             alert('Usuario borrado con exito..')
             location.reload()
            }
        });
        console.log(this.ObtenerUsuarios);
      },
      BorrarEntidad: async function (id) {
          let Aidi = parseInt(id)
      let estado = { estado: 2 };
      console.log("Hola");
        await axios.delete("http://localhost:3000/entidad/cambioEstado/"+Aidi).then((resp) => {
            if (resp.status == 204) {
               
                this.BorrarUsuarios(Aidi)
            }
        });
        console.log(this.ObtenerEntidades);
      },
  }
};
</script>


<style scoped>
    #text-title{
        margin-top: 20px;
        margin-left: 40%;
    }
</style>