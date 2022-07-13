<template>
  <v-app color="deep-purple" dark>
    <v-main>

      <!-- Comienzo navegación admin -->
      <v-app-bar color="black" dark name v-if="$route.name && $route.name.includes('admin-')">
        <v-app-bar-nav-icon @click="drawer = true" v-if="tokenAdmin"></v-app-bar-nav-icon>

        <v-toolbar-title style="margin-left: 30px">Hotel D'Lido</v-toolbar-title>

        <v-spacer></v-spacer>
        <!-- nombre de la sesión -->
        <div>
          <h3 style="color: white; margin-right: 30px" v-if="tokenAdmin">Hola, {{ usuario }}</h3>
        </div>

      </v-app-bar>



      <v-navigation-drawer v-model="drawer" absolute temporary>

        <h3 style="text-align:center; padding-top: 10px;">Hotel D'Lido Panel:</h3><br>
        <h3 style="text-align:center;">{{ObtenerRoles.nombreRol}}</h3>
        <hr>
        
        <separator />
        <v-list nav dense>
          <v-list-item-group v-model="group" active-class="deep-purple--text text--accent-4">

            <v-list-item to="inicio">
              <v-list-item-icon>
                <v-icon>mdi-home</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Inicio</v-list-item-title>
            </v-list-item>

            <v-list-item to="solicitar">
              <v-list-item-icon>
                <v-icon>mdi-home</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Solicitar caja</v-list-item-title>
            </v-list-item>
            
            <v-list-item to="reserva" v-if="ObtenerRoles.reservas == 1 && ObtenerUsuarios.estado == 1 ">
              <v-list-item-icon>
                <v-icon>mdi-alarm-light</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Aprobar Reserva</v-list-item-title>
            </v-list-item>

            <v-list-item to="habitaciones" v-if="ObtenerRoles.habitaciones == 1 && ObtenerUsuarios.estado == 1  &&  ObtenerCaja.estado!==0">
               <v-list-item-icon>
                <v-icon>mdi-bed</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Habitaciones</v-list-item-title>
            </v-list-item>

            <v-list-item to="check-in" v-if="ObtenerRoles.check_in == 1 && ObtenerUsuarios.estado == 1  &&  ObtenerCaja.estado!==0">
              <v-list-item-icon>
                <v-icon>mdi-bell</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Check-in</v-list-item-title>
            </v-list-item>

            <v-list-item to="servicios" v-if="ObtenerRoles.vista1 == 1 && ObtenerUsuarios.estado == 1 &&  ObtenerCaja.estado!==0">
              <v-list-item-icon>
                <v-icon>mdi-basket</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Servicios</v-list-item-title>
            </v-list-item>

            <v-list-item to="caja" v-if="ObtenerRoles.vista2 == 1 && ObtenerUsuarios.estado == 1">
              <v-list-item-icon>
                <v-icon>mdi-account-cash</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Caja</v-list-item-title>
            </v-list-item>

            <v-list-item to="check-out" v-if="ObtenerRoles.check_out == 1 && ObtenerUsuarios.estado == 1 &&  ObtenerCaja.estado!==0">
              <v-list-item-icon>
                <v-icon>mdi-exit-run</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Check-Out</v-list-item-title>
            </v-list-item>

            <br>
            <divider> Mantenimiento</divider>
            <hr>

            <v-list-item to="Usuarios" v-if="ObtenerRoles.listaclientes == 1 && ObtenerUsuarios.estado == 1">
              <v-list-item-icon>
                <v-icon>mdi-account</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Usuarios</v-list-item-title>
            </v-list-item>

            <v-list-item to="CrearCuenta"  v-if="ObtenerRoles.usuarios == 1 && ObtenerUsuarios.estado == 1">
             <v-list-item-icon>
                <v-icon>mdi-account</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Crear Usuarios</v-list-item-title>
            </v-list-item>

            <v-list-item to="Rol" v-if="ObtenerRoles.vista3 == 1 && ObtenerUsuarios.estado == 1 || ObtenerRoles.vista3 == 2 && ObtenerUsuarios.estado == 1 ">
              <v-list-item-icon>
                <v-icon>mdi-account-cog</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Asignar rol</v-list-item-title>
            </v-list-item>

            <v-list-item to="Clientes" v-if="ObtenerRoles.cuentas == 1 && ObtenerUsuarios.estado == 1">
              <v-list-item-icon>
                <v-icon>mdi-account-alert</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Aprobar Clientes</v-list-item-title>
            </v-list-item>

            <v-list-item to="clientes2" v-if="ObtenerRoles.aprobarclientes == 1 && ObtenerUsuarios.estado == 1">
              <v-list-item-icon>
                <v-icon>mdi-account-details</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Lista de Clientes</v-list-item-title>
            </v-list-item>

            <v-list-item to="detalleshabitacion" v-if="ObtenerRoles.editarhabitacion == 1 && ObtenerUsuarios.estado == 1">
              <v-list-item-icon>
                <v-icon>mdi-bed-empty</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Detalles de habitacion</v-list-item-title>
            </v-list-item>

            <v-list-item to="datos" v-if="ObtenerRoles.datos == 1 && ObtenerUsuarios.estado == 1">
              <v-list-item-icon>
                <v-icon>mdi-city</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Países y ciudades</v-list-item-title>
            </v-list-item>

            <v-list-item @click="logout()">
              <v-list-item-icon>
                <v-icon>mdi-logout</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Cerrar sesión</v-list-item-title>
            </v-list-item>

          </v-list-item-group>
        </v-list>
      </v-navigation-drawer>

      <!-- Final del panel admin -->

      <v-app-bar color="black" dark name v-if="$route.name && !$route.name.includes('admin-')">

        <v-toolbar-title style="margin-left: 30px">Hotel D'Lido</v-toolbar-title>

        <v-spacer></v-spacer>
        <!-- nombre de la sesión -->
        <div v-if="token">
          <h3 style="color: green">Hola {{ usuarioCliente }}</h3>
        </div>
        <v-menu bottom left>
          <template v-slot:activator="{ on, attrs }">
            <v-btn icon color="yellow" v-bind="attrs" v-on="on">
              <v-icon>mdi-dots-vertical</v-icon>
            </v-btn>
          </template>

          <v-list v-if="token">
            <v-list-item>
              <v-list-item-title>
                <v-btn @click="infoCuenta()">
                  Cuenta
                </v-btn>
              </v-list-item-title>
              <v-list-item-title>
                <v-btn @click="toggle()">
                  Logout
                </v-btn>
              </v-list-item-title>
            </v-list-item>
          </v-list>
        </v-menu>

        <template v-slot:extension>
          <v-tabs align-with-title>
            <v-tab>
              <v-btn text to="/">
                Inicio
              </v-btn>
            </v-tab>
            <v-tab>
              <v-btn text @click="habitacion()">
                Habitaciones
              </v-btn>
            </v-tab>
            <v-tab v-if="token">
              <v-btn text to="visualizar">
                Reservas
              </v-btn>
            </v-tab>
            <v-tab v-if="!token">
              <v-btn text to="/login">
                Inicio de sesión
              </v-btn>
            </v-tab>
            <v-tab v-if="!token">
              <v-menu offset-y>
                <template v-slot:activator="{ on, attrs }">
                  <v-btn text dark v-bind="attrs" v-on="on">
                    Registrar
                  </v-btn>
                </template>
                <v-list>
                  <v-list-item>
                    <v-list-item-title>
                      <v-btn text to="persona">
                        Persona
                      </v-btn>
                    </v-list-item-title>
                    <v-list-item-title>
                      <v-btn text to="/juridica">
                        Empresa
                      </v-btn>
                    </v-list-item-title>
                  </v-list-item>
                </v-list>
              </v-menu>
            </v-tab>
          </v-tabs>
        </template>
      </v-app-bar>

      <router-view />

     <v-footer style="margin-top: 50px" dark color="black" padless id="margenTexto" name
        v-if="$route.name && !$route.name.includes('admin-')">
        <v-card flat tile color="black" text-color="white" class="white--text text-center" id="margen">
          <!-- <v-container>
          <v-row justify="center">
            <v-col md="6"></v-col>
          </v-row>
        </v-container> -->
          <v-card-text>
            <v-btn v-for="icon in icons" :key="icon" class="mx-4 white--text" icon>
              <v-icon size="24px">
                {{ icon }}
              </v-icon>
            </v-btn>
          </v-card-text>

          <v-card-text class="pt-0" style="color: white">
            <v-container>
              <v-row justify="center">
                <v-col md="3">
                  <center>
                    <h2 id="margenTexto"><strong>Historia del hotel</strong></h2>
                  </center>
                  <center id="margenTexto" style="text-align: justify;">
                    Hotel D'Lido Managua se encuentra en Managua. La belleza natural del área puede apreciarse en Parque
                    Las Palmas y Parque El Carmen, mientras que Teatro Justo Rufino Garay y Palacio Nacional son lugares
                    culturales destacados.
                  </center>
                </v-col>
                <v-col md="3">
                  <center>
                    <h2 id="margenTexto" style="margin-left: 40px"><strong>Dirección</strong></h2>
                  </center>
                  <div id="margenTexto">
                    <iframe
                      src="https://www.google.com/maps/embed?pb=!1m14!1m8!1m3!1d15602.311046034112!2d-86.2926554!3d12.1410163!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x0%3A0x1bf3fc49ade10c25!2sHotel%20D&#39;Lido!5e0!3m2!1ses!2sni!4v1652311432207!5m2!1ses!2sni"
                      width="300" height="250" style="border:0;" allowfullscreen="" loading="lazy"
                      referrerpolicy="no-referrer-when-downgrade"></iframe>
                  </div>
                </v-col>
                <v-col md="3">
                  <center>
                    <h2 id="margenTexto" style="margin-left: 90px"><strong>Datos de contacto</strong></h2>
                  </center>

                  <div style="margin-left: 90px">
                    <center>
                      <h3 id="margenTexto">Dirección</h3>
                    </center>
                    <p style="margin-top:20px">Racachaca 1 al lago, por el arbolito del bolo.</p>
                  </div>

                  <div style="margin-left: 90px">
                    <center>
                      <h3 id="margenTexto">Teléfono</h3>
                    </center>
                    <p style="margin-top:20px">86821921</p>
                  </div>

                  <div style="margin-left: 90px">
                    <center>
                      <h3 id="margenTexto">Correo</h3>
                    </center>
                    <p style="margin-top:20px">HotelLido@gmail.com</p>
                  </div>

                </v-col>
              </v-row>
            </v-container>
          </v-card-text>

          <v-divider></v-divider>

          <v-card-text class="white--text" style="color: white">
            {{ new Date().getFullYear() }} — <strong>© Hotel D'Lido.</strong>

          </v-card-text>
        </v-card>
      </v-footer>
    </v-main>
  </v-app>
</template>

<script>
import axios from "axios";
export default {
  name: "App",

  data: () => ({
    icons: [
      'mdi-facebook',
      'mdi-twitter',
      'mdi-linkedin',
      'mdi-instagram',
    ],
    ObtenerUsuarios: [],
    ObtenerRoles:[],
    usuarioCliente:"",
    token: "",
    usuario: "",
    obtener: 0,
    idTipoPersona: 0,
    drawer: false,
    group: null,
    trabajador: 0,
    estado: 0,
    ObtenerCaja:{
      estado:0
    },
  }),

  created() {
    this.cuenta();
    this.Entidad();
    this.filtrar();
    this.obtenerRoles();
      this.obtenerCaja();
    

  },

  methods: {
    filtrar(){
      this.trabajador = parseInt(window.sessionStorage.getItem("seleccionado"))      
      this.obtenerUsuariosRoles();
    },
    obtenerUsuariosRoles: async function () {
        console.log("Hola");
        await axios.get("http://localhost:3000/usuariorol").then((resp) => {
            if (resp.status == 200) {
              for (let index = 0; index < resp.data.length; index++) {
                const element = resp.data[index];
                if (element.idUsuario == this.trabajador) {
                  this.ObtenerUsuarios = element;
                  console.log("soy usuario rol",this.trabajador)
                }
              }
              console.log("Yo soy el usuario: ", this.ObtenerUsuarios);
            
            }
        });
      },
      obtenerCaja: async  function(){
        await  axios.get('http://localhost:3000/caja/get/'+this.trabajador)
        .then((resp)=>{
          console.log("CAJA",resp);
          if (resp.status == 200){
            this.ObtenerCaja= resp.data
            console.log("datos de CAJA",this.ObtenerCaja);
            console.log("datos de CAJA2",this.ObtenerCaja[0].estado);
          }
        
        })
        .catch(error => {
                this.respuesta = error.message
                console.log(this.respuesta);
                this.ObtenerCaja.estado=0
               
            })
        
        },
      obtenerRoles: async function () {
        console.log("Hola");  
        await axios.get("http://localhost:3000/rol").then((resp) => {
            if (resp.status == 200) {
              for (let index = 0; index < resp.data.length; index++) {
                const element = resp.data[index];                
                if (element.idrRol == this.ObtenerUsuarios.idRol) {
                  this.ObtenerRoles = element;

                }
              }
              console.log("Yo soy el rol: ", this.ObtenerRoles);           
            }
        });
      },
    infoCuenta() {
      if (this.idTipoPersona == 1) {
        location.replace('/cuenta')
      } else if (this.idTipoPersona == 2) {
        location.replace('/cuentaJuridica')
      }
    },
    Entidad: async function () {
      let numero = window.sessionStorage.getItem("seleccionado");
      this.obtener = parseInt(numero);
      console.log("yooo", this.obtener);
      console.log("Hola");
      await axios
        .get("http://localhost:3000/entidad/" + this.obtener)
        .then((resp) => {
          console.log("respuesta",resp)
          if (resp.status == 200) {
            let entidad = resp.data;
            this.usuario = resp.data.primerNombre;
            this.idTipoPersona = entidad.idTipoPersona;
            console.log("se sobre escribe el valor", this.idTipoPersona);
          }
        });
    },

    logout: async function () {
      var logout = confirm("¿Estas seguro de salir?")

      if (logout == true) {
        window.sessionStorage.clear();
        location.replace('/panel')
      } else {
        alert("¡Puedes seguir navegando :)!")
      }
    },
    habitacion() {
      window.localStorage.setItem("categoriaHab", 0)
      location.replace("/Habitaciones")
    },
    cuenta: async function () {

      this.tokenAdmin = window.sessionStorage.getItem("authAdminToken");
      this.token = window.sessionStorage.getItem("authToken");
      this.usuarioCliente = window.sessionStorage.getItem('usuario')
    },
    toggle: async function () {
      var logout = confirm("¿Estas seguro de salir?")

      if (logout == true) {
        window.sessionStorage.clear();
        location.replace('/login')
      } else {
        alert("¡Puedes seguir navegando :)!")
      }
    }
  },

  watch: {
    group() {
      this.drawer = false
    },
  },
};
</script>

<style scoped>
#margen {
  margin-left: 40px;
}

#margenTexto {
  margin-top: 20px;
}

#user {
  margin-top: 10px;
}
</style>
