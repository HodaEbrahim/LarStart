<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header">Log In</div>
                    <div class="card-body">
                        <form @submit.prevent="logIn()">
                            <div class="modal-body">
                                <div class="form-group row">
                                    <label  class="col-md-4 col-form-label text-md-right">Email:</label>

                                    <div class="col-md-6">
                                        <input v-model="form.email" type="email" name="email"
                                               placeholder="Email Address"
                                               class="form-control" :class="{ 'is-invalid': form.errors.has('email') }">
                                        <has-error :form="form" field="email"></has-error>
                                    </div>
                                </div>

                                <div class="form-group row">
                                    <label  class="col-md-4 col-form-label text-md-right">Email:</label>

                                    <div class="col-md-6">
                                        <input v-model="form.password" type="password" name="password"
                                               placeholder="Password"
                                               class="form-control" :class="{ 'is-invalid': form.errors.has('password') }">
                                        <has-error :form="form" field="password"></has-error>
                                    </div>
                                </div>
                                <div class="form-group row mb-0">
                                    <div class="col-md-8 offset-md-4">
                                        <button type="submit" class="btn btn-primary">
                                            Log In
                                        </button>
                                    </div>
                                </div>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
            data(){
                return {
                    token: localStorage.getItem('access_token') || null,
                    users: [],
                    form: new Form({
                        password: '',
                        email: '',
                    }),
                }
            },
            methods: {

                logIn(){
                    // this.$store.dispatch('retrieveToken', {
                    //     username: this.form.email,
                    //     password: this.form.password,
                    // });
                    this.form.post('api/login').then(({ data }) => {
                       console.log(data.access_token)
                        const access_token = data.access_token;
                       localStorage.setItem('access_token', access_token)
                        window.location.href = '/lara/Larastart-master/public/home'
                    }).catch(({ err })=>{
                        console.log(err)
                    })
                }
            }
    }
</script>
