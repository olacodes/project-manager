<template>
  <div class="container pb-5">
    <clip-loader class="mt-5" :loading="loading" color="#00113d" size="60px"></clip-loader>
    <div class="card-container card my-5 shadow">
      <FlashMessage />

      <div class="login-header text-center p-3">
        <h2 class="login-text">Sign up for Project Manager</h2>
      </div>

      <form class="mx-5 pt-5" @submit.prevent="onSubmit">
        <div class="username-email">
          <div class="form-group username-container">
            <label for="Username or Email">Username </label>
            <input type="text" v-model="username" class="form-control form-control-sm" />
            <small class="form-text text-muted">Please enter your username</small>
          </div>

          <div class="divider mx-3"></div>

          <div class="form-group email-container">
            <label for="Email">Email</label>
            <input type="email" v-model="email" class="form-control form-control-sm" />
            <small class="form-text text-muted ">Please enter your Email</small>
          </div>
        </div>

        <div class="form-group">
          <label for="password">Password</label>
          <input type="text" v-model="password" class="form-control form-control-sm" />
          <small class="form-text text-muted ">password must not be less than 3 characters</small>
        </div>
        <div class="buttons">
          <button type="submit" class="btn btn-info sign-up">Sign up</button>
          <div class="divider mx-3"></div>
          <nuxt-link to="/login">
            <p class="text-muted sign-in">Already have an account? Sign in.</p>
          </nuxt-link>
        </div>
      </form>

      <div class="line-or mx-5 my-4">
        <div class="line-wrapper">
          <div class="line"></div>
        </div>
        <div class="or mx-3">OR</div>
        <div class="line-wrapper">
          <div class="line"></div>
        </div>
      </div>
      <button class="btn btn-primary mx-5 mb-5">Sign up with Facebook</button>
    </div>
  </div>
</template>

<script>
import ClipLoader from 'vue-spinner/src/ClipLoader.vue'
export default {
  components: {
    ClipLoader
  },

  data() {
    return {
      username: '',
      email: '',
      password: '',
      loading: null
    }
  },
  methods: {
    async onSubmit() {
      this.loading = 'loading'
      const { username, email, password } = this
      const payload = { username, email, password }

      let responseData = await this.$store.dispatch('auth/register', payload)

      let { status, message } = responseData

      this.loading = null
      if (status === 'success') {
        this.flashMessage.show({
          title: 'welcome back',
          status,
          message,
          icon:
            'https://res.cloudinary.com/olacode/image/upload/v1586618462/project%20manager/success-icon_b7m2vu.webp',
          clickable: true,
          blockClass: 'flash-message-class',
          positon: 'right top',
          x: 50,
          y: 600
        })
        setTimeout(() => {
          this.$router.push('/dashboard')
        }, 3000)
      } else {
        this.flashMessage.show({
          title: status,
          status,
          message,
          icon:
            'https://res.cloudinary.com/olacode/image/upload/v1586073406/project%20manager/error_qcaxmt.png',
          clickable: true,
          blockClass: 'flash-message-class',
          positon: 'right top',
          x: 50,
          y: 600
        })
      }
    }
  }
}
</script>

<style scoped>
/* deep__blue #00113d */
/* light cyan ##72f3ec */
@media screen and (min-width: 600px) {
  .card-container {
    width: 600px;
    margin-left: auto;
    margin-right: auto;
  }
}
@media screen and (min-width: 400px) {
  .username-container,
  .email-container,
  .sign-up,
  .sign-up {
    width: 40%;
    flex-grow: 1;
  }
  .username-email,
  .buttons {
    display: flex !important;
  }
}

.card-container {
  color: #00113d;
}
.login-header {
  background-color: #00113d;
  color: white;
}
.login-text {
  font-size: 22px;
}
.line-or {
  display: flex;
  justify-content: space-between;
}
.line-wrapper {
  display: inline-block;
  flex-grow: 2;
}
.line {
  border: 0.7px solid grey;
  text-align: center;
  margin-top: 12px;
}

nuxt-link,
a:hover {
  text-decoration: none;
}

.sign-in:hover {
  color: #00113d !important;
}
</style>
