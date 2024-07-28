<template>
    <div class="container-fluid">
    <div>
        <form @submit.prevent="submitForm" class="mx-auto">
            <h4 class="text danger">Register</h4>
            <div>
                <label class="form-label">Usuario</label>
                <input v-model="username" type="text" placeholder="Ingresa tu nombre de usuario" class="form-control">
                <small v-if="errors.username" class="text-danger">{{ errors.username }}</small>
            </div>
            <div>
                <label class="form-label">Ingresa Contraseña</label>
            <input v-model="password" type="password" placeholder="Ingresa tu contraseña" class="form-control">
            <small v-if="errors.password" class="text-danger">{{ errors.password }}</small>
            </div>
            <div>
                <label class="form-label">Repite tu contraseña</label>
            <input v-model="password2" type="password" placeholder="Repite tu contraseña" class="form-control">
            <small v-if="errors.password2" class="text-danger">{{ errors.password2 }}</small>
            </div>
            <div>
            <div>
                <label class="form-label">Email</label>
                <input v-model="email" type="email" placeholder="Ingresa tu email" class="form-control">
                <small v-if="errors.email" class="text-danger">{{ errors.email }}</small>
            </div>
            <div>
                <label class="form-label">Nombres</label>
            <input v-model="first_name" type="text" placeholder="Ingresa tu nombre" class="form-control">
            <small v-if="errors.first_name" class="text-danger">{{ errors.first_name }}</small>
            </div>
            <div>
                <label class="form-label">Apellidos</label>
                <input v-model="last_name" type="text" placeholder="Ingresa tu apellido" class="form-control">
                <small v-if="errors.last_name" class="text-danger">{{ errors.last_name }}</small>
            </div>
            <div>
                <label class="form-label">DNI</label>
                <input v-model="dni" type="text" placeholder="Ingresa tu DNI" class="form-control">
                <small v-if="errors.dni" class="text-danger">{{ errors.dni }}</small>
            </div>
            <div>
                <label class="form-label">Direccion</label>
                <input v-model="direccion" type="text" placeholder="Ingresa tu direccion" class="form-control">
                <small v-if="errors.direccion" class="text-danger">{{ errors.direccion }}</small>
            </div>
            <div>
                <label class="form-label">Telefono</label>
                <input v-model="telefono" type="text" placeholder="Ingresa tu telefono" class="form-control">
                <small v-if="errors.telefono" class="text-danger">{{ errors.telefono }}</small>
            </div>
            <button type="submit" class="btn btn-primary mt-4">Signup to the system</button>
            </div>
        </form>
    </div>
    </div>
</template>

<script>
import axios from 'axios';
const axiosInstance = axios.create({
  baseURL: 'http://127.0.0.1:8000/api/',
  //timeout: 5000,  // tiempo máximo de espera para la solicitud
  headers: {
    'Authorization': 'Token dcf9db5025fd0d9487a994d1a62b76abb5445970'  // ejemplo de token de acceso
  }
});
    export default{
        name:'Signup',
        data(){
            return{
                username:'',
                password:'',
                password2:'',
                email:'',
                first_name:'',
                last_name:'',
                dni:'',
                direccion:'',
                telefono:'',
                errors:{
                    email:'',
                    password:'',
                    password2:'',
                    email:'',
                    first_name:'',
                    last_name:'',
                    dni:'',
                    direccion:'',
                    telefono:'',
                    wrong_credentials:''
                }
            }
        },
        computed:{

        },
        methods:{
            isValidForm(){
                let valid=true;
                if(!this.username){this.errors.username = 'El campo no puede estar vacio'}
                else{this.errors.username = '';}

                if(!this.password){this.errors.password = 'El campo no puede estar en blanco'}
                else{this.errors.password = '';}

                if(this.password && this.password2 && this.password != this.password2)
                {this.errors.password2 = 'Password incorrecto';}
                else{this.errors.password2 = '';}

                if(!this.email){this.errors.email = 'El campo no puede estar en blanco'}
                else{this.errors.email = "";}

                if(!this.first_name){this.errors.first_name = 'El campo no puede estar vacio'}
                else{this.errors.first_name = ''}

                if(!this.last_name){this.errors.last_name = 'El campo no puede estar vacio'}
                else{this.errors.last_name = ''}

                if(!this.dni){this.errors.dni = 'El campo no puede estar vacio'}
                else{this.errors.dni = ''}

                //if(!this.telefono){this.errors.telefono = 'El campo no puede estar vacio'}
                //else{this.errors.telefono = ''}

                if(this.errors.username || this.errors.password || this.errors.password2 || this.errors.email ||
                    this.errors.first_name || this.errors.last_name || this.errors.dni || this.errors.telefono){valid = false;}

                return valid;
            },
            submitForm(){
                if(this.isValidForm){
                    axiosInstance.post('usuarios/',{
                        username:this.username, 
                        password:this.password, 
                        email:this.email, 
                        first_name:this.first_name, 
                        last_name:this.last_name, 
                        dni:this.dni, 
                        direccion:this.direccion, 
                        telefono:this.telefono
                    })
                    .then(response =>{
                        console.log(response.data.user);
                        this.username = '';
                        this.password = '';
                        this.password2 = '';
                        this.email = '';
                        this.first_name = '';
                        this.last_name = '';
                        this.dni = '';
                        this.direccion = '';
                        this.telefono = '';
                    })
                    .catch(error => {
                        console.log(error.response.data);
                        if(error.response.data.username){
                            this.errors.username = error.response.data.username.join('');
                        }
                        else{
                            this.errors.username = "";
                        }
                    })
                }

            }
            
        }
    }
</script>

<style>
    *{
    padding: 0;
    margin: 0;
    box-sizing: border-box;
   }
   body{
    background: linear-gradient(90deg, rgba(2,0,36,1) 0%, rgba(0,212,255,1) 100%);
    font-family: 'Poppins', sans-serif;
   }
   form{
    width: 24%;
    background-color: white;
    padding: 50px;
    border-radius: 20px;
    margin-top: 20px;
   }
   .btn-primary{
    width: 100%;
    border: none;
    border-radius: 50px;
    background: linear-gradient(90deg, rgba(2,0,36,1) 0%, rgba(0,212,255,1) 100%);

   }
   .form-control{
    color: rgba(0, 0, 0, .87);
    border-bottom-color: rgba(0,0,0,.42);
    box-shadow: none !important;
    border: none;
    border-bottom: 1px solid;
    border-radius: 4px 4px 0 0;
    }
    h4{
        font-size: 2rem !important;
        font-weight: 700;
    }
   .form-label{
    font-family: 800 !important;

   }
</style>
