<template>
  <v-container>
    <v-row>
      <v-col id="text-title">
        <h3>Creación de reservas </h3>
        <hr>
      </v-col>
      <v-col md="12">
        <v-simple-table height="300px">
          <template v-slot:default>
            <thead>
              <tr>
                <th class="text-left">Usuario</th>
                <th class="text-left">Tipo de habitación</th>
                <th class="text-left">Habitación</th>
                <th class="text-left">Servicio</th>
                <th class="text-left">Cantidad de personas</th>
                <th class="text-left">Estado</th>
                <th class="text-left">Fecha de creación</th>
                <th class="text-left">Fecha de inicio</th>
                <th class="text-left">Fecha de salida</th>
                <th class="text-left">Rechazar</th>
                <th class="text-left">Aprobar</th>
              </tr>
            </thead>
            <tbody>
              <tr v-if="item.estado == 1" v-for="(item, i) in reservaVisualizar" :key="i">
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
                  <div v-if="item.idservicio == null">
                    <label>
                      No seleccionado
                    </label>
                  </div>
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
                  <div v-if="item.estado == 3">probada</div>
                  <div v-if="item.estado == 4">Activa</div>
                  <div v-if="item.estado == 5">Finalizada</div>
                </td>
                <td>{{ item.created_at.substr(2, 8) }}</td>
                <td>{{ item.fechaEntrada }}</td>
                <td>{{ item.fechaSalida }}</td>

                <td>
                  <v-btn class="mx-2" v-bind="attrs" v-on="on"
                    @click="BorrarReservas(item.idreserva,) && CambioEstado(item.idhabitacion, EstadoSeleccionado)"
                   fab
                    dark small color="red">
                    <v-icon dark>
                      mdi-close
                    </v-icon>
                  </v-btn>

                </td>
                <td>
                <v-row justify="start">
                  <v-dialog v-model="dialogCrear" persistent max-width="600px">
                    <template v-slot:activator="{ on, attrs }">
                      <v-btn @click="obtenerHab(item.idhabitacion,item.idreserva,item.idservicio) && RecuperarReservas(item.idreserva)"  color="primary" dark v-bind="attrs" v-on="on">
                        Pagar
                      </v-btn>
                    </template>
                    <v-card>
                      <v-card-title>
                        <span class="text-h5">Datos de reserva</span>
                      </v-card-title>
                        <v-card-text>
                          <v-container>
                           <v-row justify="start">
                            <v-col cols="5" sm="6">
                                <label>Nombre de habitación</label>
                                <v-text-field
                                  v-model="costototal.nombrehabitacion"
                                  :items="nombrehabitacion"
                                  :rules="Rules"
                                  required
                              ></v-text-field>
                            </v-col>
                             <v-col cols="5" sm="6">
                                <label>Precio de la habitación</label>
                                <v-text-field
                                  v-model="costototal.preciohabitacion"
                                  :items="preciohabitacion"
                                  :rules="Rules"
                                  required
                              ></v-text-field>
                            </v-col>
                             <v-col cols="6" sm="6">
                                <label>Categoría de Habitación</label>
                                <v-text-field
                                  v-model="costototal.categoriahabitacion"
                                  :items="categoriahabitacion"
                                  :rules="Rules"
                                  required
                              ></v-text-field>
                            </v-col>
                             <v-col cols="6" sm="6">
                                <label>Nombre de servicio</label>
                                <v-text-field
                                  v-model="costototal.nombreservicio"
                                  :items="nombreservicio"
                                  :rules="Rules"
                                  required
                              ></v-text-field>
                                <!-- <v-col cols="12" sm="6">
                              <v-select
                                  v-model="costototal.nombreservicio"
                                  :items="Servicioshabitacion"
                                  item-text="servicio"
                                  item-value="servicio"
                                  label="servicio"
                                  :rules="Rules"
                                  required
                                ></v-select>
                             </v-col> -->
                            </v-col>
                             <v-col cols="6" sm="6">
                                <label>Precio del servicio</label>
                                <v-text-field
                                  v-model="costototal.precioservicio"
                                  :items="precioservicio"
                                  :rules="Rules"
                                  required
                              ></v-text-field>
                            </v-col>
                             <v-col cols="6" sm="6">
                                <label>Días</label>
                                <v-text-field
                                  v-model="costototal.dias"
                                  :items="dias"
                                  :rules="Rules"
                                  required
                              ></v-text-field>
                            </v-col>
                             <v-col cols="6" sm="6">
                                <label>Monto total</label>
                                <v-text-field
                                  v-model="costototal.monto"
                                  :items="monto"
                                  :rules="Rules"
                                  required
                              ></v-text-field>
                            </v-col>
                              <v-col cols="6" sm="6">
                                <label>Adelanto</label>
                                <v-text-field
                                 type="number"
                                  v-model="costototal.montopagado"
                                  :min="costototal.montopagado"
                                  :items="montopagado"
                                  :rules="Rules"
                                  required
                              ></v-text-field>
                            </v-col>
                            </v-row>
                          </v-container>
                        </v-card-text>
                          <!-- Pagarreserva()&& -->
                        <v-btn color="blue darken-1" text @click="Pagarreserva()&&Pagarmetodo()">
                          Pagar reserva
                        </v-btn>
                      <v-card-actions>
                        <v-spacer></v-spacer>
                        <v-btn color="blue darken-1" text @click="dialogCrear = false">
                          Cerrar
                        </v-btn>
                      </v-card-actions>
                    </v-card>
                  </v-dialog>
                </v-row>
                <!-- a -->
               
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
    reservaVisualizar: [],
    precio1:[],
     precio2:[],
      precio3:[],
      pagar4:{
       idusuarios:0,
       idreserva:0,
       costo:0,
       pago:0,
       motivo:"reserva",
       fecha: new Date().toISOString().substr(0, 10),
       hora: new Date().toLocaleTimeString()
      
      },
      costototal:{
      nombrehabitacion:"",
      preciohabitacion:0,
      categoriahabitacion:0,
     nombreservicio:"",
    precioservicio:"",
     dias:0,

      },

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
    EstadoSeleccionado: 0,
    usuarioBuscado: 0,

    RegistrarAcompas: {
      Nombre: "",
      Edad: 0,
      identificacion: "",
      telefono: ""
    },
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
  }),

  created() {
    this.obtenerReservas();
    this.obtenerHabitaciones();
    this.Pais();
    this.Ciudad();
    this.TipoEntidad();
    this.ActividadJuridica();
    this.obtenerHab()

  },

  methods: {
//NUEVO
 obtenerHab: async function (id,id2,id3) {
  let Aidi = parseInt(id)
   let Aidi2 = parseInt(id2)
       await axios.get("http://localhost:3000/reserva/list4/"+Aidi2).then((resp) => {
        if (resp.status == 200) {
          this.precio2 = resp.data;
          console.log("reserva", this.precio2);
          console.log("reserva", this.precio2[0].idhabitacion);
        }
      });
  console.log("id reserva", Aidi);
      await axios.get("http://localhost:3000/habitaciones/habitacion/"+ this.precio2[0].idhabitacion).then((resp) => {
        if (resp.status == 200) {
          this.precio1 = resp.data;
          console.log("habitacion", this.precio1);
        }
      });
  
      let Aidi3 = parseInt(id3)
       await axios.get("http://localhost:3000/servicios/"+Aidi3).then((resp) => {
        if (resp.status == 200) {
          this.precio3 = resp.data;
          console.log("servicio", this.precio3);
        }
      });
      console.log("precio habitacion",this.precio1[0].precio)
      console.log("precio dias",this.precio2[0].dias)
      console.log("precio servicio",this.precio3.precio) 
      this.costototal.monto = (this.precio1[0].precio * (this.precio2[0].dias+1)) + this.precio3.precio
      this.costototal.montopagado = this.costototal.monto/2
      console.log("costo reserva", this.costototal.monto)
      
      this.costototal.nombrehabitacion = this.precio1[0].nombre
      this.costototal.preciohabitacion = this.precio1[0].precio
      this.costototal.categoriahabitacion = this.precio1[0].idcategoriaHab  
      this.costototal.nombreservicio = this.precio3.servicio
      this.costototal.precioservicio = this.precio3.precio
      this.costototal.dias = this.precio2[0].dias+1
       console.log("costo reserva", this.costototal)
       let usuariocaja = parseInt(window.sessionStorage.getItem("seleccionado")) 
       await  axios.get('http://localhost:3000/caja/get/'+ usuariocaja)
        .then((resp)=>{
          console.log("CAJA",resp);
          if (resp.status == 200){
            this.ObtenerCaja= resp.data
            console.log("datos de CAJA",this.ObtenerCaja);
            console.log("estado de CAJA",this.ObtenerCaja[0].estado);
          } })
      

    },
       Pagarmetodo: async function () {
         this.pagar4.idreserva = this.precio2[0].idreserva,
         this.pagar4.costo = this.costototal.monto,
         this.pagar4.pago = this.costototal.montopagado,
         this.pagar4.saldo =  this.pagar4.costo - this.pagar4.pago,
         this.pagar4.estado = 0
         this.pagar4.idusuarios = this.precio2[0].idusuario;
         if (this.pagar4.saldo==0) {
           this.pagar4.estado = 1
         }
         
      console.log("metodo de pago" ,this.pagar4);
      await axios
        .post("http://localhost:3000/metodoPago/create", this.pagar4)
        .then((resp) => {
          if (resp.status == 201) {
            alert("El pago de la reserva se han hecho exitosamente.");
          
            // location.replace("/visualizar");
          } else if (resp.status == 201) {
            alert('Complete los campos pendientes porfavor');
          }
        });
    },


    Pagarreserva: async function (idUser) {

      console.log("vamos a pagar");
      let idcaja = this.ObtenerCaja[0].idCaja
      let montopago={monto:this.ObtenerCaja[0].monto + this.costototal.montopagado}
      console.log("pago",montopago)
      await axios .put("http://localhost:3000/caja/update/" + idcaja,montopago).then((resp) => {

        if (resp.status == 204) {
          this.compas = resp.data;
        }
        location.reload("/reserva")
      });
    },
    

// VIEJO
    RegistrarAcompas: async function () {
      let Aidi = parseInt(id)
      console.log("Hola");
      let send = {
        Nombre: this.RegistrarAcompas.Nombre,
        Edad: parseInt(this.RegistrarAcompas.Edad),
        Identificacion: this.RegistrarAcompas.identificacion,
        Telefono: this.RegistrarAcompas.telefono,

      }

      await axios
        .post("http://localhost:3000/acompanante/create", +Aidi, +send)
        .then((resp) => {
          if (resp.status == 201) {
            this.respuesta = resp.data
            console.log("entidad id", this.respuesta);
            this.Usuario();
            this.reserva();

          }
        })
        .catch(error => {
          let respu = error.message
          alert(respu);
          console.log(this.respuesta);
        })
    },
    obtenerUsuarios: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/usuario").then((resp) => {
        if (resp.status == 200) {
          this.usuariosVisualizar = resp.data;
        }
      });
    },
    obtenerCompas: async function (idUser) {
      console.log("Hola");
      await axios.get("http://localhost:3000/acompas/" + idUser).then((resp) => {
        if (resp.status == 200) {
          this.compas = resp.data;
        }
      });
    },
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
    BorrarReservas: async function (id) {
      let Aidi = parseInt(id)
      let estado = { estado: 2 };
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
        alert("Dato de reserva añadidio correctamente.");
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
  text-align: center;
  margin-top: 20px;
  font-size: 20px;
}

hr {
  border-top: 2px dashed green;
}
</style>