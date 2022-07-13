<template>
  <v-container>
<v-row>
      <v-col id="text-title">
        <h3>Check-In</h3>
      </v-col>
      <v-col md="12">
        <v-simple-table height="1000px">
          <template v-slot:default>
            <thead>
              <tr>
                <th class="text-left">Usuario</th>
                <th class="text-left">Tipo de habitación</th>
                <th class="text-left">Habitación</th>
                <th class="text-left">Servicio</th>
                <th class="text-left">Catidad de personas</th>
                <th class="text-left">Estado</th>
                <th class="text-left">Fecha de creación</th>
                <th class="text-left">Fecha de inicio</th>
                <th class="text-left">Fecha de salida</th>
                <th class="text-left">Añadir acompañantes</th>
              </tr>
            </thead>
            <tbody>
              <tr v-if="item.estado == 3" v-for="(item, i) in reservaVisualizar" :key="i">
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
                  <div v-if="item.estado == 3">probada</div>
                  <div v-if="item.estado == 4">Activa</div>
                  <div v-if="item.estado == 5">Finalizada</div>
                </td>
                <td>{{ item.created_at.substr(2, 8) }}</td>
                <td>{{ item.fechaEntrada }}</td>
                <td>{{ item.fechaSalida }}</td>

                <td>
                  <v-row justify="start">
                    <v-dialog v-model="dialogCrear1" persistent max-width="600px">
                      <template v-slot:activator="{ on, attrs }">
                        <v-btn
                          class="mx-2"
                          v-bind="attrs"
                          v-on="on"
                          v-if="item.estado == 3"
                          @click="stateRolsUsers2(item.idreserva)"
                          fab
                          dark
                          small
                          color="blue"
                        >
                          <v-icon dark> mdi-plus </v-icon>
                        </v-btn>
                        <!-- a -->

                        <v-btn
                          class="mx-2"
                          v-if="item.estado == false"
                          @click="RecuperarReservas(item.idreserva)"
                          fab
                          dark
                          small
                          color="yellow"
                        >
                          <v-icon dark> mdi-minus-circle </v-icon>
                        </v-btn>
                      </template>

                      <v-card>
                        <v-card-title>
                          <span class="text-h5">Registro de acompañantes</span>
                        </v-card-title>
                        <v-card-text>
                          <v-container>
                            <v-form v-model="validacionCreacion">
                              <v-row no-gutters>
                                                 <v-col cols="12" sm="6">
                                  <v-text-field
                                    v-model="RegistrarAcompas.Nombre"
                                    type="text"
                                    label="Nombre completo"
                                    :rules="Rules"
                                    required
                                  ></v-text-field>
                                </v-col>                             

                                <v-col cols="12" sm="6">
                                  <v-text-field
                                    v-model="RegistrarAcompas.Identificacion"
                                    type="text"
                                    label="Identificacion"
                                    :rules="Rules"
                                    required
                                  ></v-text-field>
                                </v-col>

                                <v-col cols="12" sm="6">
                                  <v-text-field
                                    v-model="RegistrarAcompas.Telefono"
                                    type="text"
                                    label="Telefono"
                                    :rules="Rules"
                                    required
                                  ></v-text-field>
                                </v-col>

                                <v-col cols="12" sm="6">
                                  <v-text-field
                                    v-model="RegistrarAcompas.Edad"
                                    type="number"
                                    label="Edad"
                                    :rules="Rules"
                                    required
                                  ></v-text-field>
                                </v-col>

                                <v-col md="3" style="margin-right: 390px">
                                  <v-btn
                                    :disabled="valid"
                                    class="mr-4"
                                    color="success"
                                    @click="registrarAcompas(RegistrarAcompas)"
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
                          <v-btn color="blue darken-1" text @click="dialogCrear1 = false && cerrar()">
                            Cerrar
                          </v-btn>
                        </v-card-actions>
                      </v-card>
                    </v-dialog>
                  </v-row>
                </td>
                 <td>
                <v-row justify="start">
                  <v-dialog v-model="dialogCrear3" persistent max-width="600px">
                    <template v-slot:activator="{ on, attrs }">
                      <v-btn @click="obtenerdatos(item.idreserva)"  color="primary" dark v-bind="attrs" v-on="on">
                        cambiar
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
                                <label>Fecha de Entrada </label>
                                <v-text-field
                                  v-model="cambio.fechaEntrada"
                                  :items="[0].fechaEntrada"
                                 
                                  item-value="fechaEntrada"
                                  :rules="Rules"
                                  required
                              ></v-text-field> 
                            </v-col>
                             <v-col cols="5" sm="6">
                                <label>Fecha de Salida</label>
                                <v-text-field
                                  v-model="cambio.fechaSalida"
                                  :items="fechaSalida"
                                  :rules="Rules"
                                  required
                              ></v-text-field>
                            </v-col>
                             <v-col cols="6" sm="6">
                                <label>Número de acompañantes</label>
                                <v-text-field
                                  v-model="cambio.cantPersonas"
                                  :items="cantPersonas"
                                  :rules="Rules"
                                  required
                              ></v-text-field>
                            </v-col>
                             <v-col cols="6" sm="6">
                                <label>Días</label>
                                <v-text-field
                                  v-model="cambio.dias"
                                  :items="dias"
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
                                <label>Habitación</label>
                                 <v-select
                                  v-model="cambio.idhabitacion"
                                  :items="habitacionesVisualizar"
                                  item-text="nombre"
                                  item-value="idhabitacion"
                                  label="nombre"
                                  :rules="Rules"
                                  required
                                ></v-select>
                                 
                            </v-col>
                            </v-row>
                          </v-container>
                        </v-card-text>
                          <!-- Pagarreserva()&& -->
                        <v-btn color="blue darken-1" text @click="cambiarreserva()">
                          Cambiar
                        </v-btn>
                      <v-card-actions>
                        <v-spacer></v-spacer>
                        <v-btn color="blue darken-1" text @click="dialogCrear3 = false">
                          Cerrar
                        </v-btn>
                      </v-card-actions>
                    </v-card>
                  </v-dialog>
                </v-row>
                <!-- a -->
               
                </td>
                 <td>
                <v-row justify="start">
                  <v-dialog v-model="dialogCrear2" persistent max-width="600px">
                    <template v-slot:activator="{ on, attrs }">
                      <v-btn @click="obtenerHab(item.idhabitacion,item.idreserva,item.idservicio)"  color="primary" dark v-bind="attrs" v-on="on">
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
                                <label>Precio de habitación</label>
                                <v-text-field
                                  v-model="costototal.preciohabitacion"
                                  :items="preciohabitacion"
                                  :rules="Rules"
                                  required
                              ></v-text-field>
                            </v-col>
                             <v-col cols="6" sm="6">
                                <label>Categoría de habitación</label>
                                <v-text-field
                                  v-model="costototal.categoriahabitacion"
                                  :items="categoriaHabitacion"
                                  item-text="titulo"
                                  item-value="titulo"
                                  required
                              ></v-text-field>
                               <v-select
                                  v-model="costototal
                                  .idhabitacion"
                                  :items="categoriaHabitacion"
                                  item-text="titulo"
                                  item-value="idhabitacion"
                                  label="categoriaHabitacion"
                                  :rules="Rules"
                                  required
                                ></v-select>
                              
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
                                <label>Nombre de habitación</label>
                                <v-text-field
                                  v-model="costototal.precioservicio"
                                  :items="precioservicio"
                                  :rules="Rules"
                                  required
                              ></v-text-field>
                            </v-col>
                             <v-col cols="6" sm="6">
                                <label>Nombre de habitación</label>
                                <v-text-field
                                  v-model="costototal.dias"
                                  :items="dias"
                                  :rules="Rules"
                                  required
                              ></v-text-field>
                            </v-col>
                             <v-col cols="6" sm="6">
                                <label>Nombre de habitación</label>
                                <v-text-field
                                  v-model="costototal.monto"
                                  :items="monto"
                                  :rules="Rules"
                                  required
                              ></v-text-field>
                            </v-col>
                              <v-col cols="6" sm="6">
                                <label>Nombre de habitación</label>
                                <v-text-field
                                  v-model="costototal.montopagado"
                                  :items="montopagado"
                                  :rules="Rules"
                                  required
                              ></v-text-field>
                            </v-col>
                            </v-row>
                          </v-container>
                        </v-card-text>
                          <!-- Pagarreserva()&& -->
                        <v-btn color="blue darken-1" text @click="Pagarreserva()&&Pagarmetodo() && BorrarReservas(item.idreserva) && CambioEstado(item.idhabitacion, EstadoSeleccionado) ">
                          Pagar reserva
                        </v-btn>
                      <v-card-actions>
                        <v-spacer></v-spacer>
                        <v-btn color="blue darken-1" text @click="dialogCrear2 = false">
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

      <!-- dialog de Creacion  de reservas -->

      <!-- Fin del dialog -->
    </v-row>
  </v-container>
</template>
<script>

import axios from "axios";
export default {
  data: () => ({
    //
    reservaVisualizar: [],
    precio1:[],
     precio2:[],
      precio3:[],
      pagar4:{
       idusuarios:0,
       idreserva:0,
       costo:0,
       pago:0,
       motivo:"check-in"
      },
      costototal:{
      nombrehabitacion:"",
      preciohabitacion:0,
      categoriahabitacion:0,
     nombreservicio:"",
    precioservicio:"",
     dias:0,

      },

    //
    reservaVisualizar: [],
    habitacionesVisualizar: [],
   
    Servicioshabitacion: [],
    usuariosVisualizar: [],
    ObtenerUsuarios: [],
    ObtenerCaja: [],
  
   
    categoriaHabitacion: [],
    equipamientoVisualizar: [],
    ciudad: [],
    tipoentidad: [],
    actividadjurid: [],
    dialogCrear1: false,
    dialogCrear2: false,
    dialogCrear3: false,
    validacionCreacion: true,
    menu: false,
    menu2: false,
    Rules: [(v) => !!v || "El campo es requerido"],
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
    //
    cambio:{
    fechaEntrada: "",
    fechaSalida: "",
    estado: 3,
    cantPersonas: 0,
    dias: 0,
    idservicio: 0,
    idhabitacion: 0,
    idusuario: 0,
    idcategoriaHab: 0,
    },
    ///
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
      nombre_caja:"",
      descripcion:"",
      fecha_apertura:"",
      cantidad_inicial:0,
      cantidad_cierre:0,
      monto:0,
      ubicacion:"",
      estado:1,
      idrol:0,
      trabajador:0
    },
    RegistrarAcompas: {
      Nombre: "",
      Edad: 0,
      Identificacion: "",
      Telefono: "",
      idreserva: 0,
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
    contador:0,
  }),

  created() {
    this.obtenerReservas();
    this.obtenerHabitaciones();
    this.Pais();
    this.Ciudad();
    this.TipoEntidad();
    this.ActividadJuridica();
    this.obtenerUsuarios();
    this.contador();
  },

  methods: {
    ///// limite acompañantes
    Contador: async function () {
      if (this.contador==3 && this.tipoha==1) {
        reload.location
        
      }
    },
    ///// editar reserva 
    obtenerdatos: async function (id) {
   let Aidi2 = parseInt(id)
   console.log("sera",Aidi2)
       await axios.get("http://localhost:3000/reserva/list4/"+Aidi2).then((resp) => {
        if (resp.status == 200) {
          let temporal = resp.data
          this.cambio = temporal[0];
           this.cambio.dias =  this.cambio.dias+1
          console.log("reserva", this.cambio);
          console.log("reserva", this.cambio.dias);
        }
      });
    },
     cambiarreserva: async function () {
  let id =this.cambio.idreserva
   console.log("sera",this.cambio)
   
       await axios.put("http://localhost:3000/reserva/update/"+ id, this.cambio).then((resp) => {
         console.log("se cambiaron los datos de reserva",resp); 
        if (resp.status == 204) {
          console.log("se cambiaron los datos de reserva",resp); 
}
      });
    },

    //// caja
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
         this.pagar4.idusuarios = this.precio2[0].idusuario;
         let costo = {costo:4000}
         let id = {idreserva: 36, motivo:"check",costo:4000}
      console.log("metodo de pago" ,this.pagar4);
      await axios
        .post("http://localhost:3000/metodoPago/update"+id)
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
    


    //// anterior
     stateRolsUsers3: async function (id) {
      this.idrol = parseInt(id);
      console.log("Id rol: ", this.idrol);
      this.obtenerRoles();     
    },

     obtenerRoles: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/caja").then((resp) => {
        if (resp.status == 200) {
          for (let index = 0; index < resp.data.length; index++) {
            const element = resp.data[index];
            if (element.idCaja == this.idrol) {
              this.cajaa2 = element;             
            }
          }
          console.log("Yo soy el rol: ", this.cajaa2);
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
        this.trabajador = parseInt(window.sessionStorage.getItem("seleccionadoAdmin"))
        await axios.get("http://localhost:3000/usuario"+this.trabajador).then((resp) => {
          if (resp.status == 200) {
            this.ObtenerUsuario = resp.data;
          }
        });

      },
    cerrar: async function () {
         location.reload("/panel/check-in");
        },
     stateRolsUsers2: async function (id) {
      this.idrol = parseInt(id);
      this.RegistrarAcompas.idreserva= this.idrol;
      console.log("Id rol: ", this.idrol);   
    },
    registrarAcompas: async function () {
      // let Aidi = parseInt(id);
      console.log("Hola");
      console.log("acompa",this.RegistrarAcompas);
      let acompa = this.RegistrarAcompas;
      
      await axios.post("http://localhost:3000/acompanante/create", acompa)
        .then((resp) => {
           console.log("acompa",this.RegistrarAcompas)
          console.log("acompa",resp)
          if (resp.status == 201) {
            this.RegistrarAcompas = resp.data;
            console.log("entidad id", this.RegistrarAcompas);
            alert("acompañante agregado exitosamente");
            
          }
         
        })
        .catch((error) => {
          let respu = error.message;
          alert(respu);
          console.log(this.respuesta);
        });
    },


      caja: async function(){
      let send ={
        idusuario:this.cajaa.idusuario,
        nombre_caja:this.cajaa.nombre_caja,
        descripcion:this.cajaa.descripcion,
        fecha_apertura:this.cajaa.fecha_apertura,
        cantidad_inicial:this.cajaa.cantidad_inicial,
        cantidad_cierre:this.cajaa.cantidad_cierre,
        monto:this.cajaa.monto,
        ubicacion:this.cajaa.ubicacion,
        estado:1
      }
      await  axios.post("http://localhost:3000/caja/create",send).then((resp) => {
        if (resp.status == 201) {
          alert("Caja creada correctamente");
          location.reload();
        }
      })
    },
  // obtenerCaja: async  function(){
  //       await  axios.get('http://localhost:3000/caja')
  //       .then((resp)=>{
  //         console.log("CAJA",resp);
  //         if (resp.status == 200){
  //           this.ObtenerCaja= resp.data
  //         }
  //       })
  //       },

     obtenerUsuarios: async function () {

        await axios.get("http://localhost:3000/usuario").then((resp) => {
          if (resp.status == 200) {
            this.ObtenerUsuarios = resp.data;
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
            this.respuesta = resp.data;
            console.log("entidad id", this.respuesta);
            this.Usuario();
          }
        })
        .catch((error) => {
          let respu = error.message;
          alert(respu);
          console.log(this.respuesta);
        });
    },
    Pais: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/pais").then((resp) => {
        if (resp.status == 200) {
          for (let index = 0; index < resp.data.length; index++) {
            const element = resp.data[index];
            this.paises.push({
              idPais: element.idPais,
              Nombre: element.Nombre,
            });
          }
        }
      });
      console.log(this.paises);
    },
    Ciudad: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/ciudad").then((resp) => {
        if (resp.status == 200) {
          this.ciudad = resp.data;
        }
      });
      console.log(this.ciudad);
    },
    TipoEntidad: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/tipoentidad").then((resp) => {
        if (resp.status == 200) {
          this.tipoentidad = resp.data;
        }
      });
      console.log(this.tipoentidad);
    },
    ActividadJuridica: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/actividadJuridica").then((resp) => {
        if (resp.status == 200) {
          this.actividadjurid = resp.data;
        }
      });
      console.log(this.actividadjurid);
    },

    // vanmos a ver si sirve xd
    CambioEstado: async function (id, estadoCambio) {
      let Aidi = parseInt(id);
      let idreserva = this.idreserva;
      let estado = { estado: "Ocupada" };

      await axios
        .put(
          "http://localhost:3000/habitaciones/cambiarEstado/" + Aidi,
          estado,
          +idreserva
        )
        .then((resp) => {
          if (resp.status == 204) {
            this.obtenerHabitaciones();
            this.obtenerReservas();
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
    // otra cosaxd
    BorrarReservas: async function (id) {
      let Aidi = parseInt(id);
      let estado = { estado: 4 };
      console.log("Hola");
      await axios
        .put("http://localhost:3000/reserva/cambioEstado/" + Aidi, estado)
        .then((resp) => {
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
      let Aidi = parseInt(id);
      let estado = { estado: 1 };

      await axios
        .put("http://localhost:3000/reserva/cambioEstado/" + Aidi, estado)
        .then((resp) => {
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
      this.reservar.cantPersonas = parseInt(this.reservar.cantPersonas);
      console.log("Hola");
      await axios
        .post("http://localhost:3000/reserva/create", this.reservar)
        .then((resp) => {
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
