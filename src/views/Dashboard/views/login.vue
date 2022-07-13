<template>
    <v-container>
        <v-row justify="center">
            <v-card
                class="mx-auto"
                max-width="700"
                outlined
                id="margen"
            >

            <center><h2 id="margen">Inicio de sesión</h2></center>
            <v-form
                ref="form"
                v-model="valid"
                lazy-validation
                id="margen"
            >
                <v-container>
                    <v-row justify="center">
                         <v-col md="5" style="margin-right: 30px">
                            <label>Correo</label>
                            <v-text-field
                                :rules="emailRules"
                                v-model="email"
                                required
                            ></v-text-field>
                        </v-col>

                        <v-col md="5">
                            <label>Contraseña</label>
                            <v-text-field
                                v-model="password"
                                :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                                :type="show1 ? 'text' : 'password'"
                                style="margin-right: 15px;"
                                :rules="passRules"
                                hint="At least 8 characters"
                                @click:append="show1 = !show1"
                            ></v-text-field>
                        </v-col>
                        <v-col md="3" style="margin-right: 390px">
                            <v-btn
                               :disabled="!valid"
                                class="mr-4"
                                color="success"
                                @click="login()"
                            >
                            Entrar
                            </v-btn>
                        </v-col>
                    </v-row>
                </v-container>
            </v-form>
            </v-card>
        </v-row>
    </v-container>
</template>

<script>
import axios from 'axios'
  export default {
    data: () => ({
      show1: false,
      valid: true,
      name: '',
      nameRules: [
        v => !!v || 'Name is required',
        v => (v && v.length <= 10) || 'Name must be less than 10 characters',
      ],
      email: '',
      passRules: [(v) => !!v || "Escribre tu contraseña"],
      emailRules: [
        v => !!v || 'El correo electronico es requerido',
        v => /.+@.+\..+/.test(v) || 'El correo electronico no es valido',
      ],
      entidades: "",
      usuarios: {},
      seleccionado:{},
      email: '',
      password: '',
    }),

    methods: {
      validate () {
        this.$refs.form.validate()
      },
      reset () {
        this.$refs.form.reset()
      },
      resetValidation () {
        this.$refs.form.resetValidation()
      },

      login: async function() {
          let send = {
                correo: this.email,
                password: this.password,
            }
            // if (this.email !== "" && this.password  !== "") {
            //     send = {
            //         correo: this.email,
            //         password: this.password,
            //     }
            //     this.reset ();
            // }else{
            //     this.validate()
            // }
            
            await axios
            .post("http://localhost:3000/authenticacionAdmin/login", send)
            .then((resp) => {
                if (resp.status == 200) {
                   this.respuesta = "Sesión abierta" 
                   this.Entidad();
                   console.log("me ejecuto")
                   this.Usuarios();
                   sessionStorage.setItem('authAdminToken', resp.data.token)
                   
                }
            })
            .catch(error => {
                this.respuesta = error.message
                console.log(this.respuesta);
                alert('Correo y/o contraseña incorrecto');
            })
        },

       Entidad: async function () {
      let numero = window.sessionStorage.getItem("seleccionado");
      this.obtener = parseInt(numero);
      console.log("yooo", this.obtener);
      console.log("Hola");
      await axios
        .get("http://localhost:3000/entidad/" )
        .then((resp) => {
          console.log("respuesta",resp)
          if (resp.status == 200) {
            let entidad = resp.data;
            this.usuario = resp.data.primerNombre;
            this.idTipoPersona = entidad.idTipoPersona;
            console.log("todo birn", this.idTipoPersona);
          }
        });
    },
         Usuarios: async function () {
             console.log('Hola');
             let usuario = this.email
            await axios
            .get('http://localhost:3000/usuario/'+usuario)
            .then((resp) => {
                console.log("a",resp)
            if (resp.status == 200) {
                let seleccionadoxd = resp.data[0].idUsuario
                window.sessionStorage.setItem("seleccionado", seleccionadoxd);
                console.log('SELECCIONADO', seleccionadoxd);
            }
            console.log(this.seleccionado);
             location.replace('/panel/dashboard')
            })
         },
    },
  }
</script>

<style scoped>
#margen{
    margin-top: 30px;
}
</style>