<template>
  <div class="container">
    <div class="row">
      <div class="col-12 col-md-8 col-lg-4 mx-auto">
        <div class="card card-default mb-3">
          <div class="card-body text-center">
            Create new Account
          </div>
        </div>
        <div class="card card-default">
          <div class="card-body">
            <div class="alert alert-danger" v-if="has_error && !success">
              <span v-if="error == 'registration_validation_error'">
                Your registration input is not valid
              </span>
              <span v-else>
                Something an error at our system
              </span>
            </div>
            <form
              autocomplete="off"
              @submit.prevent="register"
              v-if="!success"
              method="post"
            >
              <div
                class="form-group"
                v-bind:class="{
                  'has-error': has_error && errors.email
                }"
              >
                <label for="name">Username</label>
                <input
                  type="text"
                  id="name"
                  class="form-control"
                  placeholder="Username"
                  v-model="name"
                />
                <span class="help-block" v-if="has_error && errors.name">{{
                  errors.name
                }}</span>
              </div>
              <div
                class="form-group"
                v-bind:class="{
                  'has-error': has_error && errors.email
                }"
              >
                <label for="email">E-mail</label>
                <input
                  type="email"
                  id="email"
                  class="form-control"
                  placeholder="E-mail"
                  v-model="email"
                />
                <span class="help-block" v-if="has_error && errors.email">{{
                  errors.email
                }}</span>
              </div>
              <div
                class="form-group"
                v-bind:class="{
                  'has-error': has_error && errors.password
                }"
              >
                <label for="password">Password</label>
                <input
                  type="password"
                  id="password"
                  placeholder="Password"
                  class="form-control"
                  v-model="password"
                />
                <span class="help-block" v-if="has_error && errors.password">{{
                  errors.password
                }}</span>
              </div>
              <div
                class="form-group"
                v-bind:class="{
                  'has-error': has_error && errors.password
                }"
              >
                <label for="password_confirmation">Confirm Password</label>
                <input
                  type="password"
                  id="password_confirmation"
                  placeholder="Confirmation Password"
                  class="form-control"
                  v-model="password_confirmation"
                />
              </div>
              <button type="submit" class="btn btn-primary d-block mx-auto">
                Create Account
              </button>
            </form>
          </div>
        </div>
        <div class="text-center mt-4">
          <router-link to="/auth/login">
            Already have account?
          </router-link>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      name: "",
      email: "",
      password: "",
      password_confirmation: "",
      has_error: false,
      error: "",
      errors: {},
      success: false
    };
  },
  methods: {
    register() {
      // disable register
      var app = this;
      this.$auth
        .register({
          data: {
            name: app.name,
            email: app.email,
            password: app.password,
            password_confirmation: app.password_confirmation
          }
        })
        .then(
          function() {
            app.success = true;
            this.$router.push({
              name: "login",
              params: { successRegistrationRedirect: true }
            });
          },
          function(res) {
            app.has_error = true;
            app.error = res.response.data.error;
            app.errors = res.response.data.errors || {};
          }
        );
    }
  }
};
</script>
