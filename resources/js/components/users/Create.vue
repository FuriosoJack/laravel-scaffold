<template>
  <div>
    <ol class="breadcrumb">
      <li class="breadcrumb-item">
        <a href="/users">
          <i class="fa fa-chevron-left mr-1"></i>
          Users
        </a>
      </li>
      <li class="breadcrumb-item active">New</li>
      <li class="breadcrumb-menu">
        <a class="btn btn-outline-primary text-primary" href="#" :disabled="submiting" @click.prevent="create">
          <i class="fas fa-spinner fa-spin" v-if="submiting"></i>
          <i class="far fa-save d-lg-none"></i>
          <span class="d-md-down-none ml-1">Create user</span>
        </a>
      </li>
    </ol>
    <div class="container">
      <div class="card-body">
        <div class="row justify-content-md-center">
          <div class="col-md-12 col-xl-7">
            <div class="form-group">
              <label>Full Name</label>
              <input type="text" class="form-control" :class="{'is-invalid': errors.name}" v-model="user.name" placeholder="John Doe">
              <div class="invalid-feedback" v-if="errors.name">{{errors.name[0]}}</div>
            </div>
            <div class="form-group">
              <label>Email</label>
              <input type="email" class="form-control" :class="{'is-invalid': errors.email}" v-model="user.email" placeholder="john@modulr.io">
              <div class="invalid-feedback" v-if="errors.email">{{errors.email[0]}}</div>
            </div>
            <div class="form-group">
              <label>Password</label>
              <input type="password" class="form-control" :class="{'is-invalid': errors.password}" v-model="user.password">
              <div class="invalid-feedback" v-if="errors.password">{{errors.password[0]}}</div>
            </div>
            <div class="form-group">
              <label>Roles</label>
              <multiselect
                v-model="user.roles"
                :options="roles"
                :multiple="true"
                openDirection="bottom"
                track-by="id"
                label="display_name"
                :class="{'border border-danger rounded': errors.roles}">
              </multiselect>
              <small class="form-text text-danger" v-if="errors.roles">{{errors.roles[0]}}</small>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      user: {
        roles: []
      },
      roles: [],
      errors: {},
      submiting: false
    }
  },
  mounted () {
    this.getRoles()
  },
  methods: {
    create () {
      if (!this.submiting) {
        this.submiting = true
        axios.post(`/api/users/store`, this.user)
        .then(response => {
          this.$toasted.global.error('Created user!')
          location.href = '/users'
        })
        .catch(error => {
          this.errors = error.response.data.errors
          this.submiting = false
        })
      }
    },
    getRoles () {
      axios.get(`/api/roles/all`)
      .then(response => {
        this.roles = response.data
      })
      .catch(error => {
        this.errors = error.response.data.errors
      })
    }
  }
}
</script>
