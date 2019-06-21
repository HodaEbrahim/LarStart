<template>
    <div class="container">
        <div class="row mt-5">
            <div class="col-12">
                <div class="card">
                    <div class="card-header">
                        <h3 class="card-title">Users Table</h3>

                        <div class="card-tools">
                           <button @click="addModal" class="btn btn-success">Add User <i class="fas fa-user-plus fa-w-1"></i></button>
                        </div>
                    </div>
                    <!-- /.card-header -->
                    <div class="card-body table-responsive p-0">
                        <table class="table table-hover">
                            <tbody>
                            <tr>
                                <th>ID</th>
                                <th>User</th>
                                <th>Email</th>
                                <th>Role</th>
                                <th>Registered At</th>
                                <th>Modify</th>
                            </tr>
                            <tr v-for="user in users" :key="user.id">
                                <td>{{ user.id }}</td>
                                <td>{{ user.name | upLetter }}</td>
                                <td>{{ user.email }}</td>
                                <td>{{ user.role | upLetter }}</td>
                                <td>{{ user.created_at | customDate }}</td>
                                <td>
                                    <a  href="#" @click="editUser(user)">
                                        <i class="fas fa-edit">

                                        </i>
                                    </a>
                                    /
                                    <a href="#" @click="deleteUser(user.id)">
                                        <i class="fas fa-trash red fa-w-2">

                                        </i>
                                    </a>
                                </td>
                            </tr>

                            </tbody></table>
                    </div>
                    <!-- /.card-body -->
                </div>
                <!-- /.card -->
            </div>
        </div>
        <div class="modal fade" id="addModal" tabindex="-1" role="dialog" aria-labelledby="addModalLabel" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 v-show="!edit" class="modal-title" id="addModalLabel">Add user</h5>
                        <h5 v-show="edit" class="modal-title">Update user</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <form @submit.prevent="edit ? updateUser() : addUser() ">
                    <div class="modal-body">
                        <div class="form-group">
                            <input v-model="form.name" type="text" name="name"
                                   placeholder="Name"
                                   class="form-control" :class="{ 'is-invalid': form.errors.has('name') }">
                            <has-error :form="form" field="name"></has-error>
                        </div>

                        <div class="form-group">
                            <input v-model="form.email" type="email" name="email"
                                   placeholder="Email Address"
                                   class="form-control" :class="{ 'is-invalid': form.errors.has('email') }">
                            <has-error :form="form" field="email"></has-error>
                        </div>

                        <div class="form-group">
                        <textarea v-model="form.bio" name="bio" id="bio"
                                  placeholder="Short bio for user (Optional)"
                                  class="form-control" :class="{ 'is-invalid': form.errors.has('bio') }"></textarea>
                            <has-error :form="form" field="bio"></has-error>
                        </div>


                        <div class="form-group">
                            <select name="role" v-model="form.role" id="role" class="form-control" :class="{ 'is-invalid': form.errors.has('role') }">
                                <option value="">Select User Role</option>
                                <option value="admin">Admin</option>
                                <option value="user">Standard User</option>
                                <option value="author">Author</option>
                            </select>
                            <has-error :form="form" field="role"></has-error>
                        </div>

                        <div class="form-group">
                            <input v-model="form.password" type="password" name="password" id="password"
                                   placeholder="Type password" class="form-control" :class="{ 'is-invalid': form.errors.has('password') }">
                            <has-error :form="form" field="password"></has-error>
                        </div>

                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                        <button v-show="!edit" type="submit" class="btn btn-primary">Add</button>
                        <button v-show="edit" type="submit" class="btn btn-success">Save</button>
                    </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script>


    export default {
        data(){
            return {
                users: [],
                form: new Form({
                    id:'',
                    name : '',
                    email: '',
                    password: '',
                    role: '',
                    bio: '',
                    photo: ''
                }),
                pagination: {},
                edit: false
            }
        },
        methods: {
            updateUser: function(){
                this.$Progress.start();
                this.form.put('api/user/' + this.form.id).then(({ data }) => {
                    $("#addModal").modal('hide');
                    toast.fire({
                        type: 'success',
                        title: 'User updated successfully'
                    });
                    this.fetchUsers();
                    this.$Progress.finish();
                })
                    .catch(()=>{
                        this.$Progress.fail();
                    })
            },
            editUser: function(user){
              this.edit = true;
              this.showModal();
              this.form.fill(user);

              console.log(user)
              console.log(this.form)
            },
            addModal: function(){
                this.edit = false;
                this.showModal();
            },
            showModal: function(){
                $("#addModal").modal('show');
                this.form.reset();
            },
            addUser: function () {
                this.$Progress.start();
                this.form.post('api/user').then(({ data }) =>{
                    this.users = data.data;
                    $("#addModal").modal('hide');
                    toast.fire({
                        type: 'success',
                        title: 'User created successfully'
                    });
                    this.$Progress.finish();
                    })
                    .catch(()=>{
                        this.$Progress.fail();
                    })

            },
            fetchUsers: function () {
                axios.get('api/user').then(({ data }) => {
                    console.log(data);
                    this.users = data.data;
                });

            },
            deleteUser: function (id) {
                swal.fire({
                    title: 'Are you sure?',
                    text: "You won't be able to revert this!",
                    type: 'warning',
                    showCancelButton: true,
                    confirmButtonColor: '#3085d6',
                    cancelButtonColor: '#d33',
                    confirmButtonText: 'Yes, delete it!'
                }).then((result) => {
                    if (result.value) {
                        this.form.delete('api/user/' + id).then(({ data }) =>{
                            swal.fire(
                                'Deleted!',
                                'Your file has been deleted.',
                                'success'
                            )
                            this.fetchUsers();
                        }).catch(()=> {
                            swal.fire("Failed!", "There was something wronge.", "warning");
                            this.fetchUsers();
                        });
                    }
                })

            }
        },
        mounted() {
            console.log('Component mounted.')
        },
        created: function () {
            this.fetchUsers();
            //setInterval(() => this.fetchUsers(),3000);
        }
    }
</script>
