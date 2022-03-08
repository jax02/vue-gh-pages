<template>
  <div class="d-flex h-100 text-center text-dark">
    <div class="cover-container d-flex w-100 h-100 p-3 mx-auto flex-column">
      <FrontNavbar></FrontNavbar>
      <h2 class="pt-3">商品分類 Product Category</h2>
      <nav class="mb-4">
        <ul
          class="align-items-center justify-content-center nav overflow-auto row row-cols-2 row-cols-lg-6"
        >
          <li class="nav-item col">
            <div class="btn">
              <div class="card text-dark">
                <img
                  src="https://images.unsplash.com/photo-1644893198612-960a6c5d5317?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwcm9maWxlLXBhZ2V8N3x8fGVufDB8fHx8&auto=format&fit=crop&w=800&q=60"
                  class="card-img opacity-50"
                  alt=""
                />
                <div class="card-img-overlay">
                  <h5 class="card-body">衣襪服飾</h5>
                </div>
              </div>
            </div>
          </li>
          <li class="nav-item col">
            <div class="btn">
              <div class="card text-dark">
                <img
                  src="https://images.unsplash.com/photo-1644893198612-960a6c5d5317?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwcm9maWxlLXBhZ2V8N3x8fGVufDB8fHx8&auto=format&fit=crop&w=800&q=60"
                  class="card-img opacity-50"
                  alt=""
                />
                <div class="card-img-overlay">
                  <h5 class="card-body">耳環飾品</h5>
                </div>
              </div>
            </div>
          </li>
          <li class="nav-item col">
            <div class="btn">
              <div class="card text-dark">
                <img
                  src="https://images.unsplash.com/photo-1644893198612-960a6c5d5317?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwcm9maWxlLXBhZ2V8N3x8fGVufDB8fHx8&auto=format&fit=crop&w=800&q=60"
                  class="card-img opacity-50"
                  alt=""
                />
                <div class="card-img-overlay">
                  <h5 class="card-body">手帳文具</h5>
                </div>
              </div>
            </div>
          </li>
          <li class="nav-item col">
            <div class="btn">
              <div class="card text-dark">
                <img
                  src="https://images.unsplash.com/photo-1644893198612-960a6c5d5317?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwcm9maWxlLXBhZ2V8N3x8fGVufDB8fHx8&auto=format&fit=crop&w=800&q=60"
                  class="card-img opacity-50"
                  alt=""
                />
                <div class="card-img-overlay">
                  <h5 class="card-body">雜七雜八</h5>
                </div>
              </div>
            </div>
          </li>
        </ul>
      </nav>
      <div class="container">
        <!-- <select name="" class="form-select w-25 mb-3 g-0">
          <option value="全部" selected>全部</option>
          <option value="床架">T恤</option>
          <option value="收納">襪子</option>
          <option value="窗簾">耳環</option>
          <option value="床架">鑰匙圈</option>
          <option value="收納">手帳貼紙</option>
          <option value="窗簾">紙膠帶</option>
          <option value="床架">卡片</option>
          <option value="收納">明信片</option>
          <option value="窗簾">防水大貼紙</option>
        </select> -->
        <h2 class="pt-3">精選商品 Featured Products</h2>
        <div class="row row-cols-1 row-cols-lg-4 g-3">
          <div class="col" v-for="product in products" :key="product.id">
            <div class="card h-100">
              <img :src="product.imageUrl" class="card-img-top" alt="" />
              <div class="card-body">
                <h5 class="card-title">{{ product.title }}</h5>
                <p class="card-text">
                  {{ product.describtion }}
                </p>
                  <div
                class="btn-group"
                role="group"
                aria-label="Basic outlined example"
              >
                <router-link
                  class="btn btn-lg btn-danger"
                  :to="`/product/${product.id}`"
                  >產品細節</router-link
                >
                <button type="button" class="btn btn-outline-danger">
                  <i class="bi bi-bag-plus fs-5"></i>
                </button>
              </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import FrontNavbar from '@/components/FrontNavbar.vue'
export default {
  components: {
    FrontNavbar
  },
  data () {
    return {
      products: []
    }
  },
  methods: {
    getProducts () {
      this.$http(
        `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/products/all`
      )
        .then((res) => {
          this.products = res.data.products
        })
        .catch((err) => {
          console.log(err)
        })
    }
  },
  mounted () {
    this.getProducts()
  }
}
</script>
