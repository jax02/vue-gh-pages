<template>
  <h2>購物車</h2>
  <div class="container">
    <table class="table align-middle">
      <thead>
        <tr>
          <th>圖片</th>
          <th>商品名稱</th>
          <th>價格</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="product in products" :key="product.id">
          <td style="width: 200px">
            <!-- 塞背景圖的方式,html知識 -->
            <div
              :style="{ backgroundImage: `url(${product.imageUrl})` }"
              style="
                height: 100px;
                background-size: cover;
                background-position: center;
              "
            ></div>
          </td>
          <td>
            {{ product.title }}
          </td>
          <td>
            <div class="h5" v-if="product.price === product.origin_price">
              {{ product.price }} 元
            </div>
            <div v-else>
              <del class="h6">原價 {{ product.origin_price }} 元</del>
              <div class="h5">現在只要 {{ product.price }} 元</div>
            </div>
          </td>
          <td>
            <!-- loading -->
            <!-- 寫在按鈕上:  :disabled="isLoadingItem==product.id;" -->
            <!-- 寫在<i>上 v-show -->
            <div class="btn-group btn-group-sm">
              <button
                type="button"
                class="btn btn-outline-secondary"
                @click="openProductModal(product.id)"
              >
                <i
                  class="fas fa-spinner fa-pulse"
                  v-show="isLoadingItem == product.id"
                ></i>
                查看更多
              </button>
              <button
                type="button"
                class="btn btn-outline-danger"
                @click="addToCart(product.id)"
              >
                <div class="spinner-border spinner-border-sm" role="status"
                v-show="isLoadingItem == product.id"
                >
                  <span class="sr-only"></span>
                </div>
                加到購物車
              </button>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import emitter from '../libs/emitter'

export default {
  data () {
    return {
      //   cartData: {},
      products: [],
      isLoadingItem: '',
      productId: ''
      //   user: {
      //     name: "",
      //     email: "",
      //     tel: "",
      //     address: "",
      //   },
      //   message: "",
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
    getProducts () {
      this.$http
        .get(
          `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/products/all`
        )
        .then((res) => {
          this.products = res.data.products
        })
        .catch((err) => {
          console.log(err)
        })
    },
    // openProductModal(id) {
    //   //元件要加上ref才能用此方式呼叫
    //   this.productId = id;
    //   this.$refs.productModal.openModal();
    // },
    getCart () {
      this.$http(
        `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/cart`
      )
        .then((res) => {
          this.cartData = res.data.data
        })
        .catch((err) => {
          console.log(err)
        })
    },
    addToCart (id, qty = 1) {
      const data = {
        product_id: id,
        qty
      }
      this.isLoadingItem = id
      this.$http
        .post(
          `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/cart`,
          { data }
        )
        .then((res) => {
          console.log(this.isLoadingItem)
          this.getCart()
          this.isLoadingItem = ''
          emitter.emit('get-cart')
          //   this.$refs.productModal.closeModal();
        })
        .catch((err) => {
          console.log(err)
        })
    }
    // removeCartItem(id) {
    //   this.isLoadingItem = id;
    //   axios
    //     .delete(`${apiUrl}/api/${apiPath}/cart/${id}`)
    //     .then((res) => {
    //       this.getCart();
    //       this.isLoadingItem = "";
    //     })
    //     .catch((err) => {
    //       console.log(err);
    //     });
    // },
    // removeCarts() {
    //   axios
    //     .delete(`${apiUrl}/api/${apiPath}/carts`)
    //     .then((res) => {
    //       this.getCart();
    //     })
    //     .catch((err) => {
    //       console.log(err);
    //     });
    // },
    // updateCartItem(item) {
    //   const data = {
    //     product_id: item.id,
    //     qty: item.qty,
    //   };
    //   this.isLoadingItem = item.id;
    //   axios
    //     .put(`${apiUrl}/api/${apiPath}/cart/${item.id}`, { data })
    //     .then((res) => {
    //       this.getCart();
    //       this.isLoadingItem = "";
    //     })
    //     .catch((err) => {
    //       console.log(err);
    //     });
    // },
    // 手機號碼規則
    // isPhone(value) {
    //   const phoneNumber = /^(09)[0-9]{8}$/;
    //   return phoneNumber.test(value) ? true : "需要正確的電話號碼";
    // },
    // 送出表單
    // onSubmit() {
    //   const data = {
    //     user: this.user,
    //     message: this.meaasge,
    //   };
    //   axios
    //     .post(`${apiUrl}/api/${apiPath}/order`, { data })
    //     .then((res) => {
    //       //重設表單
    //       console.log(res);
    //       this.$refs.form.resetForm();
    //     })
    //     .catch((err) => {
    //       console.log(err);
    //     });
    // },
  },
  mounted () {
    this.getProducts()
    this.getCart()
  }
}
</script>
