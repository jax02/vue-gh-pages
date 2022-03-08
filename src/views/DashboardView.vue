<template>
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <div class="container-fluid">
      <router-link class="navbar-brand" to="/admin">後台</router-link>
      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarNav"
        aria-controls="navbarNav"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav">
          <li class="nav-item">
            <router-link class="nav-link" to="/admin">後台首頁</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/">前台首頁</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/admin/products"
              >產品列表</router-link
            >
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/admin/coupon"
              >優惠卷</router-link
            >
          </li>
          <li class="nav-item">
            <a href="#" class="nav-link" @click.prevent="signOut">登出</a>
          </li>
        </ul>
      </div>
    </div>
  </nav>
  <h2 v-if="checkSucess">後台登入成功</h2>
  <router-view v-if="checkSucess"></router-view>
</template>

<script>
export default {
  data () {
    return {
      checkSucess: false
    }
  },
  methods: {
    checkLogin () {
      const token = document.cookie.replace(
        /* eslint-disable */
        /(?:(?:^|.*;\s*)hexToken\s*\=\s*([^;]*).*$)|^.*$/,
        '$1'
      )
      /* eslint-enable */
      if (token) {
        this.$http.defaults.headers.common.Authorization = token
        const api = `${process.env.VUE_APP_API}api/user/check`
        this.$http
          .post(api, { api_token: this.token })
          .then(res => {
            this.checkSucess = true
          })
          .catch(err => {
            console.log(err)
            this.$router.push('/login')
          })
      } else {
        alert('請先登入')
        this.$router.push('/login')
      }
    },
    signOut () {
      document.cookie = 'hexToken=;expires=;'
      alert('token已清除')
      this.$router.push('/login')
    }
  },
  mounted () {
    this.checkLogin()
  }
}
</script>
