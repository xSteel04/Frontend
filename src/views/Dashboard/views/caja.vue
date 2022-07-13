<template>
  <v-container>
    <v-row justify="start">
     <v-dialog v-model="dialogCrear"  max-width="600px">
        <template v-slot:activator="{ on, attrs }">
          <v-btn color="primary" dark v-bind="attrs" v-on="on"> crear </v-btn>
        </template>
        <v-card>
          <v-card-title>
            <span class="text-h5">Asignar datos de caja</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-form v-model="validacionCreacion">
                <v-row no-gutters>
                  <v-card class="mx-auto" max-width="700" outlined id="margen">
                    <center>
                      <h2 id="margen">Registro de Caja</h2>
                    </center>
                    <v-form ref="form" v-model="valid" lazy-validation id="margen">
                      <v-col cols="12" sm="6">
                       <v-col cols="12" sm="6">
                       <v-select
                          v-model="cajaa.idusuario"
                          :items="ObtenerCaja"
                          item-text="nombre_caja"
                          item-value="nombre_caja"
                          label="Usuario"
                          :rules="Rules"
                          required
                        ></v-select>
                      </v-col>
                      </v-col>

                      <v-container>
                        <v-row justify="center">
                          <v-col md="5" style="margin-right: 30px">
                            <label>Nombre</label>
                            <v-text-field
                              v-model="cajaa.nombre_caja"
                              :rules="Rules"
                              required
                            ></v-text-field>
                          </v-col>
                          <v-col md="5" style="margin-right: 30px">
                            <label>Descripción</label>
                            <v-text-field
                              v-model="cajaa.descripcion"
                              :rules="Rules"
                              required
                            ></v-text-field>
                          </v-col>
                          <v-col md="5" style="margin-right: 30px">
                            <label>Fecha de apertura</label>
                            <v-text-field
                              v-model="cajaa.fecha_apertura"
                              :rules="Rules"
                              required
                            ></v-text-field>
                          </v-col>

                          <v-col md="5" style="margin-right: 30px">
                            <label>Cantidad inicial</label>
                            <v-text-field
                              v-model="cajaa.cantidad_inicial"
                              :rules="Rules"
                              required
                            ></v-text-field>
                          </v-col>

                          <v-col md="5" style="margin-right: 30px">
                            <label>Cantidad cierre</label>
                            <v-text-field
                              v-model="cajaa.cantidad_cierre"
                              :rules="Rules"
                              required
                            ></v-text-field>
                          </v-col>
                          <v-col md="5" style="margin-right: 30px">
                            <label>Monto actual</label>
                            <v-text-field v-model="cajaa.monto" :rules="Rules" required>
                            </v-text-field>
                          </v-col>

                          <v-col md="5" style="margin-right: 30px">
                            <label>Ubicación</label>
                            <v-text-field
                              v-model="cajaa.ubicacion"
                              :rules="Rules"
                              required
                            >
                            </v-text-field>
                          </v-col>

                          <v-btn
                            :disabled="!valid"
                            class="mr-4"
                            color="success"
                            @click="caja()&& correo()"
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
            <v-btn color="blue darken-1" text @click="dialogCrear = false">
              Cerrar
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>

      <v-col md="12">
        <v-col id="text-title"><h3>Estado de Caja</h3></v-col>
        <v-simple-table height="300px">
          <template v-slot:default>

            <thead>
            <tr>
              <th class="text-left">Nombre caja </th>
              <th class="text-left">Nombre de usuario </th>
              <th class="text-left">Observación</th>
              <th class="text-left">Fecha de apertura</th>
              <th class="text-left">Cantidad inicial</th>
              <th class="text-left">Cantidad de cierre</th>
              <th class="text-left">Monto actual</th>
              <th class="text-left">Ubicacion</th>
               <th class="text-left">Estado</th>
              <th class="text-left">Cierre</th>
            </tr>
            </thead>
            <tbody>

            <tr v-for="(item, i) in ObtenerCaja" :key="i">
              <td>{{ item.nombre_caja }}</td>
             <td >
                <div v-for="(itemEntid, i) in ObtenerUsuarios" :key="i">
                  <div v-if="item.idusuario == itemEntid.idUsuario">
                    {{itemEntid.nombre}}
                  </div>
                </div>
              </td>
              <td>{{ item.descripcion }}</td>
              <td>{{ item.fecha_apertura }}</td>
              <td>{{ item.cantidad_inicial }}</td>
              <td>{{ item.cantidad_cierre }}</td>
              <td>{{ item.monto }}</td>
              <td>{{ item.ubicacion }}</td>
               <td>
                    <div v-if="item.estado == 0">Desabilitada</div>
                    <div v-if="item.estado == 1">Espera</div>
                    <div v-if="item.estado == 2">Operando</div>
                    <div v-if="item.estado == 3">Cerrada</div>               
             </td>
              <td>
     <v-dialog v-model="dialogCrear2" persistent max-width="600px">
        <template v-slot:activator="{ on, attrs }">
          <v-btn  @click="stateRolsUsers2(item.idCaja)" color="primary" dark v-bind="attrs" v-on="on"> cerrar  </v-btn>
        </template>
        <v-card>
          <v-card-title>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-form v-model="validacionCreacion">
                <v-row no-gutters>
                  <v-card class="mx-auto" max-width="700" outlined id="margen">
                    <center>
                      <h2 id="margen">Registro de Caja</h2>
                    </center>
                    <v-form ref="form" v-model="valid" lazy-validation id="margen">
                      
                   
                      

                      <v-container>
                        <v-row justify="center">
                          <v-col md="5" style="margin-right: 30px">
                            <label>nombre</label>
                            <v-text-field
                              v-model="cajaa2.nombre_caja"
                                item-value="nombre_caja"
                                
                            ></v-text-field>
                          </v-col>
                          <v-col md="5" style="margin-right: 30px">
                            <label>Descripción</label>
                            <v-text-field
                              v-model="cajaa2.descripcion"
                              :rules="Rules"
                              required
                            ></v-text-field>
                          </v-col>
                          <v-col md="5" style="margin-right: 30px">
                            <label>Fecha de apertura</label>
                            <v-text-field
                              v-model="cajaa2.fecha_apertura"
                              :rules="Rules"
                              
                            ></v-text-field>
                          </v-col>

                          <v-col md="5" style="margin-right: 30px">
                            <label>Cantidad inicial</label>
                            <v-text-field
                              v-model="cajaa2.cantidad_inicial"
                              :rules="Rules"
                              required
                            ></v-text-field>
                          </v-col>

                          <v-col md="5" style="margin-right: 30px">
                            <label>Cantidad cierre</label>
                            <v-text-field
                              v-model="cajaa2.cantidad_cierre"
                              :rules="Rules"
                              required
                            ></v-text-field>
                          </v-col>
                          <v-col md="5" style="margin-right: 30px">
                            <label>Monto actual</label>
                            <v-text-field v-model="cajaa2.monto" :rules="Rules" required>
                            </v-text-field>
                          </v-col>

                          <v-col md="5" style="margin-right: 30px">
                            <label>Ubicación</label>
                            <v-text-field
                              v-model="cajaa2.ubicacion"
                              :rules="Rules"
                              required
                            >
                            </v-text-field>
                          </v-col>
                            <v-col md="5" style="margin-right: 30px">
                            <label>Estado</label>
                            <v-text-field
                              v-model="cajaa2.estado"
                              
                              :rules="Rules"
                              
                            >
                            </v-text-field>
                          </v-col>
                          <v-btn
                            :disabled="valid"
                            class="mr-4"
                            color="success"
                            @click="cambiarRol(item.idCaja,cajaa2)"
                          >
                            Cerrar Caja
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
              <th class="text-left">Nombre de usuario </th>
              <th class="text-left">Observación</th>
              <th class="text-left">Fecha de solicitud</th>
              <th class="text-left">Cantidad solicitada</th>
              <th class="text-left">Ubicación</th>
              <th class="text-left">Rechazar</th>
              <th class="text-left">Aprobar</th>
            </tr>
            </thead>
             <tbody>

            <tr v-for="(item, i) in Obtenersolicitud" :key="i" v-if="item.estado == 1 && item.fecha_apertura === date">
             <td>
                  <div v-for="(itemEntid, i) in ObtenerCaja" :key="i">
                    <div v-if="item.idCaja == itemEntid.idCaja">
                      {{itemEntid.nombre_caja}}
                    </div>
                  </div>
             </td>
             <td >
                <div v-for="(itemEntid, i) in ObtenerUsuarios" :key="i">
                  <div v-if="item.idusuario == itemEntid.idUsuario">
                    {{itemEntid.nombre}}
                  </div>
                </div>
              </td>
              <td>{{ item.descripcion }}</td>
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
                    <v-btn class="mx-2" v-bind="attrs" v-on="on" v-if="item.estado == 1"
                     @click="stateRolsUsers3(item.idsolicitud )"
                     fab dark small color="red">
                    <v-icon dark>
                     mdi-close
                    </v-icon>
                    </v-btn>
                </td>
                        <!-- a -->
                <td>
                    <v-btn class="mx-2" v-if="item.estado == 1"
                     @click="stateRolsUsers4(item.idsolicitud )" fab
                    dark small color="green">
                     <v-icon dark>
                     mdi-check
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
import axios from 'axios'
export default {
  data: () => ({
    Rules: [(v) => !!v || "El campo es requerido"],
    ObtenerUsuarios: [],
    ObtenerCaja:[],
    dialogCrear: false,
    dialogCrear2: false,
    cajaa:{
      idusuario:"",
      nombre_caja:"",
      descripcion:"",
      fecha_apertura:"",
      cantidad_inicial:0,
      cantidad_cierre:0,
      monto:0,
      ubicacion:"",
      estado:1,

    },
    ObtenerUsuario:[],
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
    Obtenersolicitud:[],
    solucitud:[],
    date: new Date().toISOString().substr(0, 10),
  }),
    created() {
      this.obtenerUsuarios();
      this.obtenerCaja();
      this.obtenersolicitud();
    },

    methods: {
        stateRolsUsers3: async function (id) {
      this.idrol = parseInt(id);
      console.log("id solicitud: ", this.idrol);
      this.Estadorechazado();     
    },
       stateRolsUsers4: async function (id) {
         this.trabajador = parseInt(window.sessionStorage.getItem("seleccionado"))
      this.idrol = parseInt(id);
      console.log("id solicitud: ", this.idrol);
      await axios.get("http://localhost:3000/solicitudcaja/list2/"+ this.idrol  ).then((resp) => {
        if (resp.status == 200) {
          this.solucitud = resp.data;
          console.log("solicitud aprobada",this.solucitud)
         
         }
      });
      
      this.obtenerCaja2();
      this.Estadoaprobado(this.solucitud);     
    },
      Estadorechazado: async function (id) {
       let estado = { 
        estado: 0,
        idusuarioaprobado:parseInt(window.sessionStorage.getItem("seleccionado"))
        };      
        await axios.put("http://localhost:3000/solicitudcaja/update/" + this.idrol, estado).then((resp) => {
         if (resp.status == 204) {
          alert("solicitud rechazada")
          location.reload();
         }
      });
      },
       Estadoaprobado: async function (solucitud) {
        
       let send ={
        
        idusuario:this.solucitud[0].idusuario,
      
        descripcion:this.solucitud[0].descripcion,
        fecha_apertura:this.solucitud[0].fecha_apertura,
        cantidad_inicial:this.solucitud[0].cantidad_inicial,
      
        
    
        estado:2
      }
      console.log("paso",send);
      await axios
        .put("http://localhost:3000/caja/update/" +this.solucitud[0].idCaja,send )
        .then((resp) => {
          if (resp.status == 204) {
            alert("solicitud aprobada")
            location.reload();
         }
         });
            let estado = { 
        estado: 2,
        idusuarioaprobado:parseInt(window.sessionStorage.getItem("seleccionado"))
        };      
        await axios.put("http://localhost:3000/solicitudcaja/update/" + this.idrol, estado).then((resp) => {
         if (resp.status == 204) {
          alert("caja lista para su uso")
          location.reload();
         }
      });
  



      },
      stateRolsUsers2: async function (id) {
      this.idrol = parseInt(id);
      console.log("Id rol: ", this.idrol);
      this.obtenerRoles();     
    },  
    
    
    correo: async function (id) {
       console.log("Yo soy el rol: ", this.cajaa2);
      let correo = {email: "asdrualezama@gmail.com"}
       await axios.post("http://localhost:3000/email",correo).then((resp) => {
      });
       
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
      let esta ={estado:1}
      
      console.log("Id Reserva: ", Aidi, "Estado: ", estado);
      await axios
        .put("http://localhost:3000/caja/update/" + this.idrol, estado,esta)
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
          }
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

          alert("Caja creada Correctamente");
          location.reload();

        }
      })

    },
      obtenerCaja: async  function(){
        console.log("fecha",this.date)
        await  axios.get('http://localhost:3000/caja')
        .then((resp)=>{
          console.log("CAJA",resp);
          if (resp.status == 200){
            this.ObtenerCaja= resp.data

          }
        })

      },
          obtenerCaja2: async  function(){
           
        await  axios.get('http://localhost:3000/caja'+ this.solucitud[0].idCaja)
        .then((resp)=>{
          console.log("CAJA",resp);
          if (resp.status == 200){
            this.ObtenerCaja= resp.data
          }
        })

      },
      obtenersolicitud: async  function(){
        await  axios.get('http://localhost:3000/solicitudcaja')
        .then((resp)=>{
          console.log("CAJA",resp);
          if (resp.status == 200){
            this.Obtenersolicitud= resp.data
          }
        })

      },

      obtenerUsuarios: async function () {

        await axios.get("http://localhost:3000/usuario").then((resp) => {
          if (resp.status == 200) {
            this.ObtenerUsuarios = resp.data;
            console.log("usuarios",this.ObtenerUsuarios)
          }
        });

      },
    },
};
</script>


<style scoped>
#margen {
  margin-top: 30px;
}
</style>
