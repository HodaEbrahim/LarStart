<style>
    .widget-user-header
    {
        background-position: center center;
        background-size: cover;
        height: 300px!important;
    }
    .widget-user .card-footer
    {
        padding-top: 10px !important;
    }

</style>
<template>
    <div class="container">
        <div class="row">
            <div class="col-md-12 my-4">
                    <!-- Widget: user widget style 1 -->
                    <div class="card card-widget widget-user">
                        <!-- Add the bg color to the header using any of the bg-* classes -->
                        <div class="widget-user-header text-white" style="background-image: url('./img/3.jpg')">
                            <h3 class="widget-user-username">Elizabeth Pierce</h3>
                            <h5 class="widget-user-desc">Web Designer</h5>
                        </div>
                        <div class="widget-user-image">
                            <img class="img-circle" src="img/1.jpg" alt="User Avatar">
                        </div>
                        <div class="card-footer">
                            <div class="row">
                                <div class="col-sm-4 border-right">
                                    <div class="description-block">
                                        <h5 class="description-header">3,200</h5>
                                        <span class="description-text">SALES</span>
                                    </div>
                                    <!-- /.description-block -->
                                </div>
                                <!-- /.col -->
                                <div class="col-sm-4 border-right">
                                    <div class="description-block">
                                        <h5 class="description-header">13,000</h5>
                                        <span class="description-text">FOLLOWERS</span>
                                    </div>
                                    <!-- /.description-block -->
                                </div>
                                <!-- /.col -->
                                <div class="col-sm-4">
                                    <div class="description-block">
                                        <h5 class="description-header">35</h5>
                                        <span class="description-text">PRODUCTS</span>
                                    </div>
                                    <!-- /.description-block -->
                                </div>
                                <!-- /.col -->
                            </div>
                            <!-- /.row -->
                        </div>
                    </div>
                    <!-- /.widget-user -->
                </div>
            <!--------------------------------------------------------->
            <div class="col-md-12">
                <div class="card">
                    <div class="card-header p-2">
                        <ul class="nav nav-pills">
                            <li class="nav-item"><a class="nav-link" href="#activity" data-toggle="tab">Activity</a></li>
                            <li class="nav-item"><a class="nav-link active" href="#settings" data-toggle="tab">Settings</a></li>
                        </ul>
                    </div><!-- /.card-header -->
                    <div class="card-body">
                        <div class="tab-content">
                            <div class="tab-pane" id="activity">
                                <!-- Post -->
                                    <h2 class="center">Display Activity</h2>
                                <!-- /.post -->
                            </div>
                            <!-- /.tab-pane -->

                            <!-- /.tab-pane -->

                            <div class="tab-pane active" id="settings">
                                <form class="form-horizontal">
                                    <div class="form-group">
                                        <label for="inputName" class="col-sm-2 control-label">Name</label>

                                        <div class="col-sm-12">
                                            <input v-model="form.name" type="email" class="form-control" id="inputName" placeholder="Name">
                                        </div>
                                    </div>
                                    <div class="form-group">
                                        <label for="inputEmail" class="col-sm-2 control-label">Email</label>

                                        <div class="col-sm-12">
                                            <input v-model="form.email" type="email" class="form-control" id="inputEmail" placeholder="Email">
                                        </div>
                                    </div>

                                    <div class="form-group">
                                        <label for="inputExperience" class="col-sm-2 control-label">Experience</label>

                                        <div class="col-sm-12">
                                            <textarea class="form-control" id="inputExperience" placeholder="Experience">{{ form.bio }}</textarea>
                                        </div>
                                    </div>
                                    <div class="form-group">
                                        <label for="inputPhoto" class="col-sm-2 control-label">Photo</label>

                                        <div class="col-sm-12">
                                            <input type="file" class="form-control" id="inputPhoto">
                                        </div>
                                    </div>
                                    <div class="form-group">
                                        <label for="inputSkills" class="col-sm-2 control-label">Passport</label>

                                        <div class="col-sm-12">
                                            <input type="text" class="form-control" id="inputSkills" placeholder="leave passport">
                                        </div>
                                    </div>

                                    <div class="form-group">
                                        <div class="col-sm-offset-2 col-sm-12">
                                            <button type="submit" class="btn btn-success">Update</button>
                                        </div>
                                    </div>
                                </form>
                            </div>
                            <!-- /.tab-pane -->
                        </div>
                        <!-- /.tab-content -->
                    </div><!-- /.card-body -->
                </div>
                <!-- /.nav-tabs-custom -->
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data(){
            return {
                form: new Form({
                    id:'',
                    name : '',
                    email: '',
                    password: '',
                    role: '',
                    bio: '',
                    photo: ''
                }),
            }
        },
        mounted() {
            console.log('Component mounted.')
        },
        created(){
            this.$Progress.start();
            axios.get('api/profile').then(({ data }) => {
                console.log(data);
                this.form.fill(data)
            }).catch((err)=>{
                this.$Progress.fail();
                console.log(err)
            })
            this.$Progress.finish();
        }
    }
</script>
