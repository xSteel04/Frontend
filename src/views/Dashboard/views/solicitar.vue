<template>
  <v-container>
    <h2 id="text-title">Caja actual</h2>

      <v-col md="12">
        <v-col id="text-title"><h3>Aperturas de Caja</h3></v-col>
        <v-simple-table height="300px" width="20px" style="text-align: center;">
          <template v-slot:default>
            <thead>
            <tr>
              <th style="text-align: center;" >Nombre de caja </th>              
              <th style="text-align: center;">Solicitar</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="(item, i) in ObtenerCaja" :key="i">
              <td v-if="item.estado ==  1">{{ item.nombre_caja }}</td>
              <td v-if="item.estado ==  1">

       <v-dialog v-model="dialogCrear2" persistent max-width="600px">
        <template v-slot:activator="{ on, attrs }">
          <v-btn  @click="stateRolsUsers3(item.idCaja)" color="primary" dark v-bind="attrs" v-on="on"> Solicitar caja </v-btn>
        </template>
        <v-card>
          <v-card-text>
            <v-container>
              <v-form v-model="validacionCreacion">
                <v-row no-gutters>
                  <v-card class="mx-auto" max-width="700" outlined id="margen">
                    <center>
                      <h2 id="margen">Solicitud de caja</h2>
                    </center>
                    <v-form ref="form" v-model="valid" lazy-validation id="margen">                      
                      <v-container>
                        <v-row justify="center">
                          <v-col md="5" style="margin-right: 30px">
                            <label>Nombre caja<h3 id="a">{{cajaa2.nombre_caja}}</h3><hr></label>
                            
                          </v-col>
                          <v-col md="5" style="margin-right: 30px">
                            <label>Observación de la caja</label>
                            <v-text-field
                              v-model="cajaa3.descripcion"
                              :rules="Rules"
                              required
                            ></v-text-field>
                          </v-col>
                          <v-col md="5" style="margin-right: 30px">
                            <label>Fecha de solicitud</label>
                            <v-text-field
                              v-model="cajaa3.fecha_apertura"
                              :rules="Rules"
                              required
                            ></v-text-field>
                          </v-col>
                          <v-col md="5" style="margin-right: 30px">
                            <label>Monto de apertura</label>
                            <v-text-field
                              v-model="cajaa3.cantidad_inicial"
                              :rules="Rules"
                              required
                            ></v-text-field>
                          </v-col>
                        
                          <v-btn
                            :disabled="!valid"
                            class="mr-4"
                            color="success"
                            @click="solicitar(item.idCaja,cajaa3)"
                          >
                            Crear Caja
                          </v-btn>
                        </v-row>
                      </v-container>
                    </v-form>
                  </v-card>
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
     </td>

            </tr>
            </tbody>
          </template>
        </v-simple-table>
      </v-col>
       <v-col md="12">
       <v-col id="text-title"><h3>Solicitudes de Caja</h3></v-col>
        <v-simple-table height="400px">
          <template v-slot:default>

          <thead>
            <tr>
              <th class="text-left">Nombre caja </th>
              <th class="text-left">Fecha solicitud</th>
              <th class="text-left">Cantidad solicitada</th>
              <th class="text-left">Ubicación</th>
              <th class="text-left">Estado</th>
            </tr>
            </thead>
             <tbody>
   

            <tr v-for="(item, i) in Obtenersolicitud" :key="i">
             <td>
                  <div v-for="(itemEntid, i) in ObtenerCaja" :key="i">
                    <div v-if="item.idCaja == itemEntid.idCaja">
                      {{itemEntid.nombre_caja}}
                    </div>
                  </div>
             </td>
              <td>{{ item.fecha_apertura }}</td>
              <td>{{ item.cantidad_inicial }}</td>
              <td>
                  <div v-for="(itemEntid, i) in ObtenerCaja" :key="i">
                    <div v-if="item.idCaja == itemEntid.idCaja">
                      {{itemEntid.ubicacion}}
                    </div>
                  </div>
             </td>
              <td>
                    <div v-if="item.estado == 0">Rechazada</div>
                    <div v-if="item.estado == 1">Espera</div>
                    <div v-if="item.estado == 2">Aprobada</div>
                    <div v-if="item.estado == 3">Cerrada</div>               
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
    reservaVisualizar: [],
    habitacionesVisualizar: [],
    categoriaHabitacion: [],
    Servicioshabitacion: [],
    usuariosVisualizar: [],
    ObtenerUsuarios: [],
    usuarios:[],
    ObtenerCaja: [],
    habitacionesVisualizar: [],
    categoriaHabitacion: [],
    equipamientoVisualizar: [],
    ciudad: [],
    tipoentidad: [],
    actividadjurid: [],
    dialogCrear: false,
    dialogCrear2: false,

    validacionCreacion: true,
    menu: false,
    menu2: false,
    Rules: [(v) => !!v || "El campo es requerido"],

    cajaa: {
      idusuario: "",
      nombre_caja: "",
      descripcion: "",
      fecha_apertura: "",
      cantidad_inicial: 0,
      cantidad_cierre: 0,
      monto: 0,
      ubicacion: "",
      estado: 1,
    },
      cajaa2:{
      idusuario:"",
      idCaja:"",
      descripcion:"todo esta correcto",
      fecha_apertura:"",
      cantidad_inicial:0,      
      ubicacion:"",
      estado:1,
      idrol:0,
      trabajador:0
    },
    cajaa3:{
      idusuario:"",
      idCaja:"",
      descripcion:"todo esta correcto",
      fecha_apertura:new Date().toISOString().substr(0, 10),
      cantidad_inicial:0,      
      ubicacion:"",
      estado:1,
      idrol:0,
      trabajador:0
    },

    
   
    Rules: [(v) => !!v || "Campo requerido"],
    email: "",
    emailRules: [
      (v) => !!v || "E-mail is required",
      (v) => /.+@.+/.test(v) || "E-mail must be valid",
    ],

    natural: {
      primerNombre: "",
      segundoNombre: null,
      primerApellido: null,
      segundoApellido: "",
      fechaNacimiento: null,
      genero: "",
      telefono: " ",
      celular: " ",
      correo: "acompañanteeeeee@gmail.com",
      direccion: "mi casa",
      identificacion: "",
      tipoIdentificacion: 1,
      razonSocial: null,
      siglas: null,
      ruc: null,
      codigoPostal: null,
      idPais: 1,
      idCiudad: 1,
      idActividad: null,
      idTipoPersona: 1,
    },

    
    paisId: 0,
    paises: [],
    genero: [{ genero: "M" }, { genero: "F" }],
    tipoIdent: [{ tipo: "Cedula" }, { tipo: "Pasaporte" }],
    password: "",
    confirmPassword: "",
    respuesta: {},
    respuesta2: "",
    token: "",
    usuario: "",
    firstname: "",
    lastname: "",
    obtener: 0,
    idTipoPersona: 0,
    valid: false,   
    drawer: false,
    group: null,
    menu: false,
    modal: false,
    menu2: false,
    show1: false,
    show2: false,
    trabajador: 0,
    EstadoSeleccionado: "",
    buscar: "",
    estado: "Ocupada",
    idrol: 0,
    trabajador:0,
    Obtenersolicitud:[],
  }),

  created() {
    this.obtenerUsuario();
    this.obtenerUsuarios();
    this.obtenerCaja();
    this.obtenersolicitud();
    
  },

  methods: {
     obtenersolicitud: async  function(){
        await  axios.get('http://localhost:3000/solicitudcaja/list3/'+this.trabajador)
        .then((resp)=>{
          console.log("solicitud",resp);
          if (resp.status == 200){
            this.Obtenersolicitud= resp.data
          }
        })

      },

     stateRolsUsers3: async function (id) {
      this.idrol = parseInt(id);

      console.log("Id caja: ", this.idrol);

      this.obtenerusuarioCaja();     
    },
     obtenerusuarioCaja: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/caja").then((resp) => {
        if (resp.status == 200) {
          for (let index = 0; index < resp.data.length; index++) {
            const element = resp.data[index];
            if (element.idCaja == this.idrol) {
              this.cajaa2 = element;             
            }
          }
          console.log("Yo soy la caja: ", this.cajaa2);
          this.obtenerUsuario();
        }
      });
    },
    cambiarRol: async function (id) {
      let Aidi = parseInt(id);
      let estado = this.cajaa2;
      console.log("Id Reserva: ", Aidi, "Estado: ", estado);
      await axios
        .put("http://localhost:3000/caja/update/" + this.idrol, estado)
        .then((resp) => {
          if (resp.status == 204) {
            location.reload();
          }
        });
    },
    obtenerUsuario: async function () {
        this.trabajador = parseInt(window.sessionStorage.getItem("seleccionado"))
        await axios.get("http://localhost:3000/usuario"+this.trabajador).then((resp) => {
          if (resp.status == 200) {
            this.ObtenerUsuario = resp.data;
            console.log("usuario",this.ObtenerUsuario)
          }
        });
      },
       obtenerUsuarios: async function () {
        let trabajo = this.cajaa2.idusuario;
        await axios.get("http://localhost:3000/usuario/list2"+trabajo).then((resp) => {
          if (resp.status == 200) {
            this.ObtenerUsuarios = resp.data;
            console.log("usuario",this.ObtenerUsuarios)
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

      caja: async function(){
      let send ={
        idusuario:this.cajaa3.idusuario,
        nombre_caja:this.cajaa3.nombre_caja,
        descripcion:this.cajaa3.descripcion,
        fecha_apertura:this.cajaa3.fecha_apertura,
        cantidad_inicial:this.cajaa3.cantidad_inicial,
        cantidad_cierre:this.cajaa3.cantidad_cierre,
        monto:this.cajaa3.monto,
        ubicacion:this.cajaa3.ubicacion,
        estado:1
      }
      await  axios.post("http://localhost:3000/caja/create",send).then((resp) => {
        if (resp.status == 201) {
          alert("Caja creada Correctamente");
          location.reload();
        }
      })
    },
     solicitar: async function(){
      let send ={
        idusuario:this.trabajador,
        idCaja: this.idrol,
        fecha_apertura:this.cajaa3.fecha_apertura,
        cantidad_inicial:this.cajaa3.cantidad_inicial,
        descripcion:this.cajaa3.descripcion,
        estado: 1,
      }
      console.log("solicitud",send)
      await  axios.post("http://localhost:3000/solicitudcaja/create",send).then((resp) => {
        if (resp.status == 201) {
          alert("solicitid creada Correctamente");
          location.reload();
        }
      })
    },
  obtenerCaja: async  function(){
        await  axios.get('http://localhost:3000/caja')
        .then((resp)=>{
          console.log("CAJA",resp);
          if (resp.status == 200){
            this.ObtenerCaja= resp.data
          }
        })
        },
    // otra vez vamos a ver xd  
  },
  setup() {},
};
</script>

<style scoped>
#a{
  padding-top: 14%;
}

#text-title {
  margin-top: 20px;
  margin-left: 40%;
}
</style>
