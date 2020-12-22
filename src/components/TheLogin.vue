<template> 
    <div class="container"> 
        <div class="row">
            <div class="col-lg-6 offset-lg-3 col-sm-10 offset-sm-1">
                <form 
                    class="text-center border border-primary p-5" 
                    style="margin-top:70px;height:auto;padding-top:100px !important;color:black;" 
                    @submit.prevent="loginUser" > 
                    <h1 class="h3 mb-3 font-weight-normal " style="text-align: center "> Inicia sesión</h1> 
                    <input 
                        type="text" 
                        id="email" 
                        class="form-control mb-5" 
                        placeholder="Email" 
                        v-model="login.email" /> 
                    <!-- Password --> 
                    <input 
                        type="password" 
                        id="password" 
                        class="form-control mb-5" 
                        placeholder="Contraseña"
                        v-model="login.password" /> 
                        <!-- inicio sesion button --> 
                    <center>
                        <button class="btn btn-primary btn-block w-75 my-4" type="submit"> Iniciar sesión </button>
                    </center> 
                </form>
            </div>
        </div> 
    </div> 
</template> 
<script> 
    import swal from "sweetalert";
    import axios from 'axios';
    export default { 
        data() { 
            return { 
                login: { 
                    email: "", 
                    password: "" 
                    } 
                }; 
            }, 
        methods: { 
             loginUser() { 
                try { 
                    axios.post('https://articulosback.herokuapp.com/api/usuario/login', this.login)
                    .then( response =>{
                        
                        let token = response.data.tokenReturn;
                        let usuario = response.data.user;
                        if (token) { 
                         localStorage.setItem("token", token);
                         localStorage.setItem('usuario',JSON.stringify(usuario));
                         swal("Exitoso", "login exitoso", "success");
                        this.$router.push("/home"); 
                        }
                       
            
                    })
                 
                    .catch(err =>{// eslint-disable-line no-unused-vars
                        swal("Oops!", "Usuario o contraseña incorrectas!", "error");
                    })
      }catch(err) { // eslint-disable-line no-unused-vars
            swal("Oops!", "Algo malo ocurrió", "error");
          }
                    
                    
                    
            
                    
                   
                   
                } 
                } 
            }; 
</script>