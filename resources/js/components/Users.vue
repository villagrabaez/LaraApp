<template>
  <div class="container">
    <div class="row mt-5">
      <div class="col-md-12">
        <div class="card">
          <div class="card-header">
            <h3 class="card-title">Users Table</h3>
            <div class="card-tools">
              <button type="button" class="btn btn-success" data-toggle="modal" data-target="#addModal">
                Add New <i class="fas fa-user-plus fa-fw"></i>
              </button>
            </div>
          </div>
          <!-- /.card-header -->
          <div class="card-body table-responsive p-0">
            <table class="table table-hover text-nowrap">
              <thead>
                <tr>
                  <th>ID</th>
                  <th>Name</th>
                  <th>Email</th>
                  <th>Type</th>
                  <th>Registered At</th>
                  <th>Modify</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="user in users" :key="user.id">
                  <td>{{ user.id }}</td>
                  <td>{{ user.name }}</td>
                  <td>{{ user.email }}</td>
                  <td>{{ user.type | upText }}</td>
                  <td>{{ user.created_at | myDate }}</td>
                  <td>
                    <a href="#">Edit 
                      <i class="text-yellow fa fa-edit"></i>
                    </a>
                    |
                    <a href="#">Delete 
                      <i class="text-red fa fa-trash"></i>
                    </a>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          <!-- /.card-body -->
        </div>
        <!-- /.card -->
      </div>
    </div>

    <!-- Modal -->
    <div class="modal fade" id="addModal" tabindex="-1" aria-labelledby="addModalLabel" aria-hidden="true">
      <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="addModalLabel">Modal title</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <form @submit.prevent="createUser">
            <div class="modal-body">
              <div class="form-group">
                <label for="name">Name</label>
                <input v-model="form.name" type="text" name="name" id="name" placeholder="Name"
                  class="form-control" :class="{ 'is-invalid': form.errors.has('name') }">
                <has-error :form="form" field="name"></has-error>
              </div>

              <div class="form-group">
                <label for="email">Email</label>
                <input v-model="form.email" type="email" name="email" id="email" placeholder="Email"
                  class="form-control" :class="{ 'is-invalid': form.errors.has('email') }">
                <has-error :form="form" field="email"></has-error>
              </div>

              <div class="form-group">
                <label for="bio">Bio</label>
                <textarea v-model="form.bio" type="text" name="bio" id="bio" placeholder="Bio"
                  class="form-control" :class="{ 'is-invalid': form.errors.has('bio') }">
                </textarea>
                <has-error :form="form" field="bio"></has-error>
              </div>

              <div class="form-group">
                <label for="type">Type</label>
                <select name="type" v-model="form.type" id="type" class="form-control" :class="{ 'is-invalid': form.errors.has('type') }">
                  <option value="">Select User Role</option>
                  <option value="admin">Admin</option>
                  <option value="user">Standar</option>
                  <option value="author">Author</option>
                </select>
                <has-error :form="form" field="type"></has-error>
              </div>

              <div class="form-group">
                <label for="password">Password</label>
                <input v-model="form.password" type="password" name="password" id="password" placeholder="Password"
                  class="form-control" :class="{ 'is-invalid': form.errors.has('password') }">
                <has-error :form="form" field="password"></has-error>
              </div>
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
              <button type="submit" class="btn btn-primary">Save changes</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        users: {},
        form: new Form({
          name: '',
          email: '',
          password: '',
          type: '',
          bio: '',
          photo: '',
        }),
      }
    },
    methods: {
      loadUsers() {
        axios.get('api/user').then(({ data }) => (this.users = data.data));
      },
      createUser() {
        this.$Progress.start();
        this.form.post('api/user')
        .then(() => {
                  // Fire.$emit('AfterCreate');

          $('#addModal').modal('hide')
          swal("Good job!", "User created successfull!", "success");
        })
        .catch((error) => {
          console.error(error);
        })
        this.$Progress.finish();
      }
    },
    created() {
      this.loadUsers();
      // Enviar un request cada 3seg.
      setInterval(() => this.loadUsers(), 3000);

      // INVESTIGAR

      // Fire.$on('AfterCreate', () => {
      //   this.loadUsers();
      // });
    }
  }
</script>