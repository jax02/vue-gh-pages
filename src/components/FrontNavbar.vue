<template>
  <link
    rel="stylesheet"
    href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.8.1/font/bootstrap-icons.css"
  />
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark sticky-top">
    <div class="container-fluid">
      <router-link class="navbar-brand" to="/">下酒菜製造所</router-link>
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
      <div
        class="collapse navbar-collapse justify-content-center"
        id="navbarNav"
      >
        <ul class="navbar-nav">
          <li class="nav-item">
            <router-link class="nav-link" to="/">首頁</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/Products">所有商品</router-link>
          </li>
          <!-- <li class="nav-item">
            <router-link class="nav-link" to="/cart">購物車</router-link>
          </li> -->
          <li class="nav-item">
            <router-link class="nav-link" to="/cart">狗狗專區</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/admin">後台首頁</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/login">登入後台</router-link>
          </li>
        </ul>
      </div>
      <i class="bi bi-search fs-6 mx-1"></i>
      <i class="bi bi-person fs-6 mx-1"></i>
      <i class="bi bi-cart4 fs-6 mx-1"></i>
      <span class="badge rounded-pill bg-danger">{{ cartData.carts.length }}</span>
    </div>
  </nav>
</template>

<script>
import emitter from '../libs/emitter'
export default {
  data () {
    return {
      cartData: {
        carts: []
      }
    }
  },
  // 用區域元件方式註冊驗證元件
  // components: {
  //     //自訂名稱:前方解構的
  //     Vform: Form,
  //     Vfield: Field,
  //     ErrorMessage: ErrorMessage,
  // },
  methods: {
    getCart () {
      this.$http
        .get(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`)
        .then((res) => {
          this.cartData = res.data.data
        })
        .catch((err) => {
          console.log(err)
        })
    }
  },
  mounted () {
    this.getCart()
    emitter.on('get-cart', () => {
      this.getCart()
    })
  }
}
</script>
