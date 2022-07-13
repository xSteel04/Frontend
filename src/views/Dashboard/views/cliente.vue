<template>
  <v-container>
    <v-row>
        <v-col md="6" id="text-title"><label>Aprobar usuario</label></v-col>
      <v-col md="12">
          <v-simple-table height="600px">
        <template v-slot:default>
          <thead>
            <tr>
              <th class="text-left">Nombre</th>
              <th class="text-left">Email</th>
              <th class="text-left">Entidad</th>
              <th class="text-left">Estado</th>
              <th class="text-left">Aprobar</th>
              <th class="text-left"> Ver informacion</th>
            </tr>
          </thead>
          <tbody v-for="(item, i) in ObtenerUsuarios" :key="i">
            <tr v-if="item.estado == 0 ">
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
                        v-if="item.estado == false"
                        @click="stateRolsUsers(item.idUsuario, estadoActivado)"
                        fab
                        dark
                        small
                        color="green"
                        >
                        <v-icon dark>
                            mdi-check
                        </v-icon>
                    </v-btn>

              </td>
              

              <td v-if="item.estado == 0">
                <v-dialog v-model="dialogCrear"  max-width="600px">
                    <template v-slot:activator="{ on, attrs }">
                      <v-btn
                      @click="stateRolsUsers3(item.idEntidad)"
                        v-bind="attrs" v-on="on"
                        class="mx-2"
                        fab
                        dark
                        small
                        color="blue"
                        >
                        <v-icon dark>
                            mdi-account
                        </v-icon>
                    </v-btn>
                    </template>
                    <v-card>
                      <v-card-title>
                        <span style="padding-left: 30%;"> Informacion de usuario </span>
                      </v-card-title>
                      <v-card-text>
                        <v-container>
                          <v-form>
                            
                        <v-row no-gutters justify="center" id="margen">
                    <v-col md="5">
                          <label >Primer Nombre:</label>
                          <h4>{{ natural.primerNombre }}</h4>
                          <hr>
                          
                      </v-col>
                      <v-col
                          md="5"
                      >
                          <label >Segundo Nombre:</label>
                          <h4>{{ natural.segundoNombre }}</h4>
                          <hr>
                          
                      </v-col>

                      <v-col
                          md="5"
                      >
                          <label>Primer Apellido:</label>
                          <h4>{{ natural.primerApellido }}</h4>
                          <hr> 
                      
                      </v-col>
                      <v-col
                          md="5"
                      >
                          <label>Segundo Apellido:</label>
                          <h4>{{ natural.segundoApellido }}</h4>
                          <hr>
                          
                      </v-col>
                      <v-col
                          md="5"
                      >
                          <label>Género:</label>
                          <h4>{{ natural.genero }}</h4>
                          <hr>
                      </v-col>

                      <v-col
                          md="5"
                      >
                          <label>Teléfono:</label>
                          <h4>{{ natural.telefono }}</h4>
                          <hr>
                      </v-col>
                      <v-col
                          md="5"
                      >
                          <label>Celular:</label>
                          <h4> {{ natural.celular }}</h4>
                          <hr>
                      </v-col>

                      

                      <v-col
                          md="5"
                      >
                          <label>Identificación:</label>
                          <h4> {{ natural.identificacion }} </h4>
                          <hr>
                      </v-col>

                      <v-col
                          md="5"
                      >
                          <label>Dirección:</label>
                          <h4> {{ natural.direccion }} </h4>
                          <hr>
                      </v-col>

                      <v-col
                          md="5"
                      >
                          <label>País:</label>
                          <h4> {{ natural.idPais }} </h4>
                        <hr>
                      </v-col>

                      <v-col
                          md="5"
                      >
                          <label>Ciudad:</label>
                          <h4> {{ natural.idCiudad }} </h4>
                        <hr>
                      </v-col>
                      
                      <v-col md="5">
                          <label>Código postal:</label>
                          <h4> {{ natural.codigoPostal }} </h4>
                          <hr>
                      </v-col>
                      <v-col md="5">
                      </v-col>
                
                    </v-row>
                          </v-form>
                        </v-container>
                      </v-card-text>
                      <v-card-actions>
                        <v-spacer></v-spacer>
                        <v-btn color="blue darken-1" text @click="dialogCrear = false" >
                          Cerrar
                        </v-btn>
                      </v-card-actions>
                    </v-card>
                </v-dialog>
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
import { async } from "regenerator-runtime";
export default {
  data: () => ({
     dialogCrear: false,
      ObtenerUsuarios: [],
      ObtenerEntidades: [],
      estadoActivado: true,
      estadoDesactivado: false,


    paises:[],
    ciudad:[],
    tipoentidad: [],

    genero: [{ genero: "M" }, { genero: "F" }],

    natural: {
      primerNombre: "",
      segundoNombre: "",
      primerApellido: "",
      segundoApellido: "",
      fechaNacimiento: new Date(
        Date.now() - new Date().getTimezoneOffset() * 60000
      )
        .toISOString()
        .substr(0, 10),
      genero: "",
      telefono: "",
      celular: "",
      correo: "",
      direccion: "",
      identificacion: "",
      tipoIdentificacion: "",
      razonSocial: null,
      siglas: null,
      ruc: null,
      codigoPostal: "",
      idPais: "",
      idCiudad: "",
    },

  }),

  created(){
      this.obtenerUsuarios();
      this.Pais(),
      this.Ciudad();
  },

  methods:{

       ///aprobar usuario
       correo: async function (email) {
                    let correo = {email: email , htmlBody:"su sulicitud de una cuenta se termino de configurar su cuenta tiene como usuario su correo"}
                    await axios.post("http://localhost:3000/email",correo).then((resp) => {
                        alert("se aprobo el usuario")
                    });
                },
      ///

    stateRolsUsers3: async function (idEntidad) {
      this.idrol = parseInt(idEntidad);
      console.log("Id entidad: ", this.idrol);
      this.obtenerUsuarioAprobar();     
    },

     obtenerUsuarioAprobar: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/entidad/obtener/" + this.idrol).then((resp) => {
        this.natural = resp.data;
        console.log("sirve")
        if (resp.status == 200) {
          for (let index = 0; index < resp.data.length; index++) {
            const element = resp.data[index];
            if (element.idEntidad == this.idrol) {
              this.natural = element;             
            }
          }
          console.log("Yo soy la entidad: ", this.natural);
          this.obtenerEntidad();
        }
      });
    },
    
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
        console.log("obtenerUsuarios");
        await axios.get("http://localhost:3000/usuario").then((resp) => {
            if (resp.status == 200) {
            this.ObtenerUsuarios = resp.data;
            this.obtenerEntidad()
            }
        });
        console.log(this.ObtenerUsuarios);
      },

      obtenerEntidad: async function () {
        console.log("obtenerEntidad");
        await axios.get("http://localhost:3000/entidad/obtener" + this.idrol).then((resp) => {
            if (resp.status == 200) {
            this.ObtenerEntidades = resp.data;
            console.log("asd", this.ObtenerEntidades);
            
            }
        });
      },      
    
    TipoEntidad: async function () {
      console.log("TipoEntidad");
      await axios.get("http://localhost:3000/tipoentidad").then((resp) => {
        if (resp.status == 200) {
          this.tipoentidad = resp.data;
        }
      });
      console.log(this.tipoentidad);
    },
    Pais: async function () {
             console.log('Pais');
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
         Ciudad: async function () {
             console.log('Ciudad');
            await axios
            .get('http://localhost:3000/ciudad')
            .then((resp) => {
            if (resp.status == 200) {
                this.ciudad = resp.data
            }
            })
            console.log(this.ciudad);
         },
    ActividadJuridica: async function () {
      console.log("ActividadJuridica");
      await axios
        .get("http://localhost:3000/actividadJuridica")
        .then((resp) => {
          if (resp.status == 200) {
            this.actividadjurid = resp.data;
          }
        });
      console.log(this.actividadjurid);
    },
  }
};
</script>

<style scoped>

#text-title{
  margin-top: 20px;
  margin-left: 40%;
}

#margen{
  padding-top: 2%;
}

h4{
  padding-top: 5%;
}

</style>