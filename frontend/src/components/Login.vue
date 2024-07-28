<template>
    <div class="container-fluid">
    <div class="login-form-container">
        <form @submit.prevent="submitForm" class="login-form mx-auto">
            <h4 class="text-center">Inicio de Sesion</h4>
            <div class="mb-3 mt-5">
            <label class="form-label">Email</label>
            <input v-model="email" type="email" placeholder="Ingresa tu email" class="form-control">
            <small v-if="errors.email" class="text-danger">{{ errors.email }}</small>
            </div>
            <div class="mb-3">
            <label class="form-label">Password</label>
            <input v-model="password" type="password" placeholder="Ingresa tu contraseña" class="form-control">
            <small v-if="errors.password" class="text-danger">{{ errors.password }}</small>
            </div>
            <div>
            <button type="submit" class="btn btn-primary mt-4">Ingresar</button>
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
        name: 'Login',
        data(){
            return{
                email:'',
                password:'',
                errors:{
                    email:'',
                    password:'',
                    wrong_credentials:''
                }
            }
        },
        computed:{
            
        },
        methods:{
            isValidForm(){
                let valid=true;
                if(!this.email){
                    this.errors.email = 'El campo nop puede ser blanco'
                }else{
                    this.errors.email = "";
                }
                if(!this.password){
                    this.errors.password = 'El campo no puede estar en blanco'
                }else{
                    this.errors.password = '';
                }
                if(this.errors.email || this.errors.password){
                    valid = false;
                }
                return valid;
            },
            submitForm(){
                if(this.isValidForm()){
                    axiosInstance.post('login/',{email:this.email, password:this.password}, {withCredentials:true})
                    .then(response => {
                        // Verifica los datos de la respuesta
                        console.log(response.data.user); // Debería mostrar el objeto usuario completo
                        console.log(response.data.token); // Debería mostrar el token

                        // Asegúrate de que response.data.user tiene el id
                        const user = response.data.user;
                        const token = response.data.token;
                        
                        if (user && user.id && token) {
                        // Llama a la mutación con un objeto que contenga tanto el token como el id
                        this.$store.commit('setToken', { token, user_id: user.id });
                        this.$router.push('/');
                        } else {
                        console.error("User ID or token is missing in the response.");
                        }
                    })
                    .catch(error => {
                        console.log(error);
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
    width: 40%;
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
