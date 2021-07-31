<template>
    <div class="container">
        <div class="justify-content-center d-flex align-items-center vh-100">
            <div class="col-md-5 col-sm-12">
                <div class="w-100">
                    <v-card :loading="isLoading">
                        <template slot="progress">
                            <v-progress-linear
                                color="bg-success"
                                height="5"
                                indeterminate
                            ></v-progress-linear>
                        </template>
                        <div class="card-body p-4">
                            <p class="text-center">Sign up to start your session</p>

                            <form class="p-1" @submit.prevent="register">
                                <div class="form-group mt-2">
                                    <label for="name">Name:</label>
                                    <input id="name" v-model="form.name" type="text" class="form-control" placeholder="Name">
                                    <InlineError v-if="errors" :errors="errors" field="name" />
                                </div>
                                <div class="form-group mt-3">
                                    <label for="email">Email Address:</label>
                                    <input id="email" v-model="form.email" type="email" class="form-control" placeholder="Email Address">
                                    <InlineError v-if="errors" :errors="errors" field="email" />
                                </div>

                                <div class="form-group mt-3">
                                    <label for="password">Password:</label>
                                    <input id="password" v-model="form.password" type="password" class="form-control" placeholder="Password" autocomplete="current-password">
                                    <InlineError v-if="errors" :errors="errors" field="password" />
                                </div>

                                <div class="form-group my-3">
                                    <label for="password_confirmation">Confirm Password:</label>
                                    <input id="password_confirmation" v-model="form.password_confirmation" type="password" class="form-control" placeholder="Confirm Password">
                                </div>

                                <div class="d-flex justify-content-between">
                                    <nuxt-link to="/login">Login</nuxt-link>
                                    <button v-if="!isLoading" type="submit" class="btn btn-success text-light">Register</button>
                                    <button v-else type="button" class="btn btn-success" disabled>Loading</button>
                                </div>
                            </form>
                        </div>
                        <!-- /.login-card-body -->
                    </v-card>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Register',
    layout:'empty',
    middleware:'guest',
    data(){
        return {
            form: {
                name:'',
                email: '',
                password: '',
                password_confirmation:'',
            },
            errors: [],
            isLoading : false
        }
    },
    head:{
        title: 'Register'
    },
    methods:{
        register(){
            this.isLoading = true;
            this.$axios.$post('/auth/register',this.form).then((response) =>{
                this.isLoading = false;
                this.$auth.loginWith('local',{data:{email:this.form.email,password:this.form.password}})
            }).catch((error)=>{
                this.isLoading = false;
                if(error.response.status === 422){
                    this.errors = this.$validationError(error.response)
                }
                this.$notifier.snackBar(error.response.data.message, 'bg-danger')
                console.log(this.errors)
            })
        }
    }

}
</script>