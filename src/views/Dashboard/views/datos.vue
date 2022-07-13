<template>
  <v-container>
    <v-row>
        <v-col md="6" id="text-title"><h3>Asignar datos al sistema</h3></v-col>

        <v-row justify="start">
        <v-dialog v-model="dialogCrear" persistent max-width="600px">
          <template v-slot:activator="{ on, attrs }">
            <v-btn color="primary" dark v-bind="attrs" v-on="on">
             Añadir ciudad
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">Asignar datos de ciudad</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-form v-model="validacionCreacion">
                  <v-row no-gutters>
                    <v-col cols="12" sm="6">
                      <v-select
                       style="margin-right: 15px;"
                        v-model="create.idPais"
                        :rules="Rules"
                        :items="paises"
                        item-text="Nombre"
                        item-value="idPais"
                        required
                      ></v-select>
                    </v-col>
                      <v-col cols="12" sm="6">
                    <label>Nombre</label>
                    <v-text-field
                        v-model="create.nombre"
                        :rules="Rules"
                        required
                    ></v-text-field>
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
                        @click="createciudad()"
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
    
        <v-dialog v-model="dialogCrear2" persistent max-width="600px">
          <template v-slot:activator="{ on, attrs }">
            <v-btn color="primary" dark v-bind="attrs" v-on="on">
             Añadir Pais
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">Asignar datos de país</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-form v-model="validacionCreacion2">
                  <v-row no-gutters>
                   
                     <v-col
                    md="5"
                >
                    <label>Nombre</label>
                    <v-text-field
                        v-model="create2.Nombre"
                         :items="boolean"
                          item-text="label"
                          item-value="condicional"
                           label="visualizar"
                        :rules="Rules"
                        required
                    ></v-text-field>

                </v-col>

                    <v-col md="3" style="margin-right: 390px">
                      <v-btn
                        @click="createpais()"
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
      </v-row>
      <v-col md="12">
          <v-simple-table height="600px">
        <template v-slot:default>
          <thead>
            <tr>
              <th class="text-left">Nombre</th>
              <th class="text-left">País</th>
              <th class="text-left"></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, i) in ciudad" :key="i" >
            
              <td>{{item.nombre}}</td>
                
            <td >
                <div v-for="(itemEntid, i) in paises" :key="i">
                  <div v-if="item.idPais == itemEntid.idPais">
                    {{itemEntid.Nombre}} 
                  </div>
                </div>
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
                        <v-icon dark>
                            mdi-account-convert
                        </v-icon>
                    </v-btn>

                    <v-btn
                        class="mx-2"
                      
                        @click="stateRolsUsers(item.idUsuarioRol, estadoActivado)"
                        fab
                        dark
                        small
                        color="green"
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
    
     ciudad: [],
     paises: [],
    
      dialogCrear: false,
      dialogCrear2: false,

      validacionCreacion: true,
       validacionCreacion2: true,


      Rules: [(v) => !!v || "El campo es requerido"],

    
      create: {
        idPais: 0,
        nombre: "",       
      },
        create2: {
        Nombre: "",       
      }
  }),

  created(){
    this.Pais();
    this.Ciudad();
 
  },

  methods:{

    stateRolsUsers: async function (id, estadoCambio) {
          let Aidi = parseInt(id)
          let estado = {estado: estadoCambio};
          console.log("Id Reserva: ", Aidi, "Estado: ", estado);
        await axios.put("http://localhost:3000/usuariorol/cambioEstado/"+Aidi, estado).then((resp) => {
            if (resp.status == 204) {
              location.reload();
            }
        });
      },

    createciudad: async function (){
      await axios
        .post("http://localhost:3000/ciudad/create", this.create)
        .then((resp) => {
          if (resp.status == 201) {
            alert("La ciudad fue creada correctamente");
            location.reload();
          }
        });
    },
    
    createpais: async function (){
      await axios
        .post("http://localhost:3000/pais/create", this.create2)
        .then((resp) => {
          if (resp.status == 201) {
            alert("El país fue creado correctamente");
            location.reload();
          }
        });
    },

        Ciudad: async function () {
             console.log('Hola');
            await axios
            .get('http://localhost:3000/ciudad')
            .then((resp) => {
            if (resp.status == 200) {
                this.ciudad = resp.data
            }
            })
            console.log(this.ciudad);
         },
          Pais: async function () {
             console.log('Hola');
            await axios
            .get('http://localhost:3000/pais')
            .then((resp) => {
            if (resp.status == 200) {
                for (let index = 0; index < resp.data.length; index++) {
                    const element = resp.data[index];
                    this.paises.push({
                        idPais: element.idPais,
                        Nombre: element.Nombre
                    });
                }
            }
            })
            console.log(this.paises);
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
  }
};
</script>


<style scoped>
    #text-title{
        margin-top: 20px;
        margin-left: 40%;
    }
</style>