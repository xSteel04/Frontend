<template>
  <v-container>
    <v-row>
      <v-col md="6" id="text-title">
        <h3>Check-Out</h3>
      </v-col>
      <v-col md="12">
        <v-simple-table height="300px">
          <template v-slot:default>
            <thead>
              <tr>
                <th class="text-left">Usuario</th>
                <th class="text-left">Tipo habitación</th>
                <th class="text-left">Habitación</th>
                <th class="text-left">Servicio</th>
                <th class="text-left">Cantidad de personas</th>
                <th class="text-left">Estado</th>
                <th class="text-left">Fecha de creación</th>
                <th class="text-left">Fecha de inicio</th>
                <th class="text-left">Fecha de salida</th>
                <th class="text-left">Finalizar reserva</th>
              </tr>
            </thead>
            <tbody>
              <tr v-if="item.estado == 4" v-for="(item, i) in reservaVisualizar" :key="i">
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
                      {{ itemServ.servicio }}
                    </div>
                  </div>
                </td>
                <th>{{ item.cantPersonas }}</th>
               <td>
                  <div v-if="item.estado == 1">Espera</div>
                  <div v-if="item.estado == 2">Rechazada</div>
                  <div v-if="item.estado == 3">Aprobada</div>
                  <div v-if="item.estado == 4">Activa</div>
                  <div v-if="item.estado == 5">Finalizada</div>
                </td>
                <td>{{ item.created_at.substr(2, 8) }}</td>
                <td>{{ item.fechaEntrada }}</td>
                <td>{{ item.fechaSalida }}</td>

                <td>
                  <v-row justify="start">
                    <v-dialog v-model="dialogCrear" persistent max-width="700px">
                      <template v-slot:activator="{ on, attrs }">

                        <v-btn class="mx-2" v-bind="attrs" v-on="on"    
                          fab dark small color="red"
                           @click="obtenerre(item.idreserva)"
                          >
                          <v-icon dark>
                            mdi-checkbox-marked-outline
                          </v-icon>
                        </v-btn>
                        <!-- a -->

                      </template>
                      <v-card>
                        <v-card-title>
                          <span class="text-h5">Gastos </span>
                        </v-card-title>
                        <v-card-text>
                          <v-container>
                        <v-simple-table>
                          <thead>
                            <tr>
                             
                              <th class="text-left">razon</th>
                              <th class="text-left">costo</th>
                              <th class="text-left">pago</th>
                              <th class="text-left">saldo</th>
                              <th class="text-left">fecha</th>
                              <th class="text-left">hora</th>
                            </tr>
                          </thead>
                          <tbody>
                            <tr v-for="(item, i) in factura" :key="i">
                              <td>{{ item.motivo }}</td>
                              <td>{{ item.costo }}</td>
                              <td>{{ item.pago }}</td>
                              <td>{{ item.saldo }}</td>
                              <td>{{ item.fecha }} </td>
                              <td>{{ item.hora }} </td>
                            </tr>
                            <td>
                            pago: {{total}}
                            </td>
                            <td>
                            iva: 15%
                            </td>
                            <tr>
                            <h3>descuesto:</h3>
                            <td>
                             <v-checkbox
                             v-model="checkbox1"
                               :label="`festivo`">
                             </v-checkbox>
                            </td>
                            <td>
                             <v-checkbox 
                               v-model="checkbox2"
                               :label="`frecuencia`">
                             </v-checkbox>
                            </td>
                              
                            </tr>
                            <td>
                            total de pago:{{totaliva}}
                            </td>
                          </tbody>
                        </v-simple-table>
                          </v-container>
                          <v-btn color="primary" @click="check()"> cobrar </v-btn>
                        </v-card-text>
                        <v-card-actions>
                          <v-spacer></v-spacer>
                          <v-btn color="blue darken-1" text @click="dialogCrear = false">
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

      <!-- dialog de Creacion  de reservas -->


      <!-- Fin del dialog -->
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  data: () => ({
    checkbox1:false,
    checkbox2:false,
    reservaVisualizar: [],
    habitacionesVisualizar: [],
    categoriaHabitacion: [],
    Servicioshabitacion: [],
    usuariosVisualizar: [],
    dialogCrear: false,
    dialogCrear2: false,
    validacionCreacion: true,
    menu: false,
    menu2: false,
    Rules: [
      v => !!v || 'El campo es requerido',
    ],
    reservar: {
      fechaEntrada: "",
      fechaSalida: "",
      cantPersonas: 0,
      estado: 0,
      idservicio: 0,
      idhabitacion: 0,
      idcategoriaHab: 0,
      idusuario: 0,
    },
    ciudad: [],
    tipoentidad: [],
    actividadjurid: [],
    valid: false,
    firstname: '',
    lastname: '',
    Rules: [
      v => !!v || 'Campo requerido',
    ],
    email: '',
    emailRules: [
      v => !!v || 'E-mail is required',
      v => /.+@.+/.test(v) || 'E-mail must be valid',
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
      idTipoPersona: 1
    },

    menu: false,
    modal: false,
    menu2: false,
    show1: false,
    show2: false,
    paisId: 0,
    paises: [],
    genero: [
      { genero: "M" },
      { genero: "F" }
    ],
    tipoIdent: [
      { tipo: "Cedula" },
      { tipo: "Pasaporte" },

    ],

    password: "",
    confirmPassword: "",

    respuesta: {},
    respuesta2: "",
    ObtenerUsuarios: [],
    token: "",
    usuario: "",
    obtener: 0,
    idTipoPersona: 0,
    drawer: false,
    group: null,
    trabajador: 0,

    habitacionesVisualizar: [],
    categoriaHabitacion: [],
    equipamientoVisualizar: [],
    EstadoSeleccionado: "",
    buscar: '',
    estado: "Ocupada",
    id:0,
    factura:[],
    total:0,
      motivo:"motivo",
    totaliva:0,
    fecha: new Date().toISOString().substr(0, 10),
    hora: new Date().toLocaleTimeString(),
    idreserva:[],
    contador:1
  
  }),

  created() {
    this.obtenerReservas();
    this.obtenerHabitaciones();
    this.Pais();
    this.Ciudad();
    this.TipoEntidad();
    this.ActividadJuridica();

  },

  methods: {
    ////obtener id
    obtenerre: async function (id) {

       let usuariocaja = parseInt(window.sessionStorage.getItem("seleccionado")) 
       await  axios.get('http://localhost:3000/caja/get/'+ usuariocaja)
        .then((resp)=>{
          console.log("CAJA",resp);
          if (resp.status == 200){
            this.ObtenerCaja= resp.data
            console.log("datos de CAJA",this.ObtenerCaja);
            console.log("estado de CAJA",this.ObtenerCaja[0].estado);
          } })
      this.id = id
      console.log("resrva",this.id)
         await axios
        .get("http://localhost:3000/metodoPago/"+this.id)
        .then((resp) => {
          console.log("check",resp)
          if (resp.status == 200) {
            this.factura = resp.data
            console.log("entidad id", this.factura);
            for (let index = 0; index < resp.data.length; index++) {
             this.total=this.total+this.factura[index].saldo  
            }
             for (let index = 0; index < resp.data.length; index++) {
             this.motivo=this.motivo+"," +this.factura[index].motivo
             console.log("motivo",this.factura[index].motivo )
              console.log("motivo",this.motivo) 
            }
             for (let index = 0; index < resp.data.length; index++) {
             console.log("id metod",this.factura[index].idmetodo )
             this.contador = index 
             console.log("id contador",this.contador )
            }
              for (let index = 0; index < resp.data.length; index++) {
             console.log("id metod",this.factura[index].idmetodo )
             this.contador = index 
             console.log("id contador",this.contador )
            }
             console.log("total",this.total)
             this.totaliva =  this.total+ this.total*0.15
          }
        })
        await axios.get("http://localhost:3000/reserva/list4/"+this.id).then((resp) => {
        if (resp.status == 200) {
          this.idreserva = resp.data;
          console.log("reserva", this.idreserva);
          console.log("reserva", this.idreserva[0].idhabitacion);
        }
      });
    },
    ////
    ////cierre
     check: async function () {
     ///inicia cambio metodo de pago
      for (let index = 0; index <= this.contador; index++) {
             console.log("id metod",this.factura[index].idmetodo )
              let cambio = {estado:1, saldo:0}
              await axios
        .put("http://localhost:3000/metodoPago/update/"+this.factura[index].idmetodo ,cambio)
        .then((resp) => {
          console.log("respuesta",resp )

        })
            
            }
        ///inicia creacion de factura
      let factu = {
        idreserva: this.id,
        idcaja:this.ObtenerCaja[0].idCaja,
        idusuario:this.ObtenerCaja[0].idusuario,
        fecha:this.fecha,
        hora: this.hora,
        pago:this.total,
        iva: 15,
        descuento:0,
        totalpago:this.totaliva,
        concepto:this.motivo
      }
     console.log("factura",factu)
       await axios
        .post("http://localhost:3000/factura/create/", factu)
        .then((resp) => {
          if (resp.status == 201) {
            this.respuesta = resp.data
            console.log("en", this.respuesta);
          }
        })
        ///inicia update caja
      console.log("vamos a pagar");
      let idcaja = this.ObtenerCaja[0].idCaja
      let montopago={monto:this.ObtenerCaja[0].monto + this.totaliva}
      console.log("pago",montopago)
      await axios .put("http://localhost:3000/caja/update/" + idcaja,montopago).then((resp) => {

        if (resp.status == 204) {
          this.compas = resp.data;
        }
        // location.reload("/reserva")
      })
      ///inicia cambio de estado de reserva
      let Aidi = this.id
      let estado = { estado: 5 };
      console.log("Hola");
      await axios.put("http://localhost:3000/reserva/cambioEstado/" + Aidi, estado).then((resp) => {
    
      })
      ///inicia cambio de estado habitacion
      let Aidi2 = this.idreserva[0].idhabitacion
      let estado2 = { estado: 'Limpieza' };

      await axios.put("http://localhost:3000/habitaciones/cambiarEstado/" + Aidi2, estado2).then((resp) => {
        if (resp.status == 204) {
         
        }
      })
        

    },
    ////
    // otra vez vamos a ver xd
    EntidadNatural: async function () {
      console.log(this.natural);
      console.log(this.password);
      await axios
        .post("http://localhost:3000/entidad/create/", this.natural)
        .then((resp) => {
          if (resp.status == 201) {
            this.respuesta = resp.data
            console.log("entidad id", this.respuesta);
            this.Usuario();

          }
        })
        .catch(error => {
          let respu = error.message
          alert(respu);
          console.log(this.respuesta);
        })
    }, Pais: async function () {
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
    TipoEntidad: async function () {
      console.log('Hola');
      await axios
        .get('http://localhost:3000/tipoentidad')
        .then((resp) => {
          if (resp.status == 200) {
            this.tipoentidad = resp.data
          }
        })
      console.log(this.tipoentidad);
    },
    ActividadJuridica: async function () {
      console.log('Hola');
      await axios
        .get('http://localhost:3000/actividadJuridica')
        .then((resp) => {
          if (resp.status == 200) {
            this.actividadjurid = resp.data
          }
        })
      console.log(this.actividadjurid);
    },

    // vanmos a ver si sirve xd
    CambioEstado: async function (id, estadoCambio) {
      let Aidi = parseInt(id)
      let estado = { estado: 'Ocupada' };
      
      await axios.put("http://localhost:3000/habitaciones/cambiarEstado/" + Aidi, estado).then((resp) => {
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
    // otra cosaxd
    checkinReservas: async function (id) {
      let Aidi = parseInt(id)
      let estado = { estado: 4 };
      console.log("Hola");
      await axios.put("http://localhost:3000/reserva/cambioEstado/" + Aidi, estado).then((resp) => {
        if (resp.status == 204) {
          this.obtenerReservas();
        }
        if (resp.status == 204) {
          this.obtenerReservas();
        }
      });
      console.log(this.ObtenerEntidades);
    },
    RecuperarReservas: async function (id) {
      let Aidi = parseInt(id)
      let estado = { estado: 3 };

      await axios.put("http://localhost:3000/reserva/cambioEstado/" + Aidi, estado).then((resp) => {
        if (resp.status == 204) {
          this.obtenerReservas();
        }
      });
      console.log(this.ObtenerEntidades);
    },

    enviarReservas: async function () {
      if (this.reservar.fechaEntrada == "") {
        alert("Complete Las fechas de la reserva.");
        return 0;
      }
      // if (this.fechaSalida == "") {
      //   alert("Complete Las fechas de la reserva.");
      //   return 0;
      // }
      if (this.reservar.idusuario == "") {
        alert("Dato de reserva añadido correctamente.");
        return 0;
      }
      this.reservar.cantPersonas = parseInt(this.reservar.cantPersonas)
      console.log("Hola");
      await axios.post("http://localhost:3000/reserva/create", this.reservar).then((resp) => {
        if (resp.status == 201) {
          alert("Reserva hecha correctamente.");
          location.reload();
        }
      });
      console.log(this.reservar);
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

    obtenerTipoHabitaciones: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/categoriaHab").then((resp) => {
        if (resp.status == 200) {
          this.categoriaHabitacion = resp.data;
        }
      });
      console.log(this.categoriaHabitacion);
    },

    obtenerHabitaciones: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/habitaciones/obtener").then((resp) => {
        console.log("hola",resp)
        if (resp.status == 200) {
          this.habitacionesVisualizar = resp.data;
        }
      });
      console.log(this.habitacionesVisualizar);
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
  },
  setup() {

  },
};
</script>


<style scoped>
#text-title {
  margin-top: 20px;
  margin-left: 40%;
}
</style>