<template>
  <div>
    <Header/>
    <br><br><br>
    <div v-if="totalQuantity==0" class="text-center"><h1>Your Basket Is Empty</h1>
      <router-link to="/products">
        <button class="btn"> Keep Exploring</button>
      </router-link>
    </div>
    <div v-else>
      <div v-for="product in products" :key="product.id">
        <div class="wrapper">
          <div class="container">
            <div class="row text-center">
              <img :src="'http://localhost/LAB11/public/images/' + product.image" width="10%" height="10%" alt=""/>
              <h4><strong>{{ product.name }} x{{ product.quantity }} {{ product.price }}$</strong></h4>
              <p style="text-align: center;">
                <button @click="remove(product.id)" class="btn">Remove one</button>
                <button @click="add(product.id)" class="btn ">Add Another</button>
              </p>
            </div>
          </div>
        </div>
      </div>
      <div class="container">
        <div class="row text-center">
          <h4><strong>{{ totalQuantity }} Items Total Price:{{ totalPrice }}$</strong></h4>
          <p style="text-align: center;">
            <router-link to="/products">
              <button class="btn"> Keep Exploring</button>
            </router-link>
            <button @click="clearBasket()" class="btn">Empty Basket</button>
            <button v-if="userLoggedIn"  @click="placeOrder()" class="btn ">Place Order</button>
            <router-link v-else to="/login">
              <button class="btn"> Login to place order</button>
            </router-link>
          </p>
        </div>
      </div>
    </div>
    <Footer/>
  </div>
</template>

<script>
import Footer from '@/components/Footer.vue'
import Header from '@/components/Header.vue'

export default {
  components: {
    Footer,
    Header
  },
  data() {
    return {
      user: {
        id: '',
        name: '',
        email: '',
        session_id: ''
      },
      products: [],
      totalPrice: 0,
      totalQuantity: 0
    }
  },
  mounted() {
    this.getProducts();
    this.getTotalPrice();
    this.getTotalQuantity();
  },
  methods: {
    clearBasket() {
      this.$store.commit('basket/clearBasket')
      this.getProducts();
      this.getTotalPrice();
      this.getTotalQuantity();
    },
    async placeOrder() {
      let data =  {status_id: 1, totalAmount: this.totalPrice, items: this.products};
      if (await this.$store.dispatch('orders/addOrder', data)) {
        this.clearBasket()
        await this.$router.push('/message/3')
      }
    },
    getProducts() {
      this.products = this.$store.getters['basket/getProducts']
      for (let i = 0; i < this.products.length; i++) {
        let product = this.$store.getters['products/getProduct'](this.products[i].id)
        this.products[i].image = product.image;
        this.products[i].name = product.name;
        this.products[i].price = product.price;
      }
    },
    getTotalPrice() {
      this.totalPrice = 0;
      for (let i = 0; i < this.products.length; i++) {
        this.totalPrice += parseFloat(this.products[i].price) * parseFloat(this.products[i].quantity);
      }
    },
    getTotalQuantity() {
      this.totalQuantity = 0;
      for (let i = 0; i < this.products.length; i++) {
        this.totalQuantity += parseInt(this.products[i].quantity);
      }
    },
    remove(productId) {
      this.$store.commit('basket/decrementProduct', productId)
      this.totalPrice -= parseFloat(this.$store.getters['products/getProduct'](productId).price)
      this.totalQuantity--
    },
    add(productId) {
      this.$store.commit('basket/incrementProduct', productId)
      this.totalPrice += parseFloat(this.$store.getters['products/getProduct'](productId).price)
      this.totalQuantity++
    },
    getUser() {
      this.user = this.$store.getters['user/getUser']
    }
  },
  computed: {
    userLoggedIn: function () {
      this.getUser()
      for (let i in this.user) return true
      return false
    }
  },
}
</script>

<style scoped>
</style>