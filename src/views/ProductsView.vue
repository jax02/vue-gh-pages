<template>
  <h2>產品列表</h2>
  <div class="container">
        <select name="" class="form-select w-25 mb-3 g-0">
            <option value="全部" selected>全部</option>
            <option value="床架">床架</option>
            <option value="收納">收納</option>
            <option value="窗簾">窗簾</option>
        </select>
    <div class="row row-cols-1 row-cols-lg-4 g-3">
      <div class="col" v-for="product in products" :key="product.id">
        <div class="card h-100">
          <img :src="product.imageUrl" class="card-img-top" alt="" />
          <div class="card-body">
            <h5 class="card-title">{{ product.title }}</h5>
            <p class="card-text">
              {{ product.describtion }}
            </p>
            <router-link class="btn btn-danger" :to="`/product/${product.id}`"
              >產品細節</router-link
            >
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
