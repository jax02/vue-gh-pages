<template>
  <h2>單一產品細節頁面</h2>
  <div class="container">
      <h3>產品細節 Details</h3>
      <div class="row">
        <div class="col-lg">
          <img :src="product.imageUrl" alt="">
        </div>
        <div class="col-lg">
          <h3>{{ product.title }}</h3>
          <p>{{ product.description }}</p>
          <p>價錢： NT$ {{ product.price }}</p>
          <p><span>{{ product.category }}</span></p>
        </div>
      </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      product: {}
    }
  },
  methods: {
    getProduct () {
      const { id } = this.$route.params
      this.$http(
        `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/product/${id}`
      )
        .then((res) => {
          this.product = res.data.product
        })
        .catch((err) => {
          console.log(err)
        })
    }
  },
  mounted () {
    this.getProduct()
  }
}
</script>
