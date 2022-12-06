<template>
  <div>


    <Header/>
    <div v-for="order in orders" :key="order.id">
      <h4><strong>Name{{ order.user.name }} {{ order.totalPrice }}$</strong></h4>
      <div v-for="product in order.products" :key="product.id">
        <div class="wrapper">
          <div class="container">
            <div class="row text-center">
              {{ Product.quantity }} {{ Product.name }}x
            </div>
          </div>
        </div>
      </div>
      Order total : {{ order.totalPrice }}
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
      isHidden: false,
      id: 0,
      user: {
        id: '',
        name: '',
        email: '',
        session_id: ''
      },
      orders:
          [
            //{
            //"id":"11",
            //"customer_id":"20",
            //"created_at":"2021-12-03 18:20:31",
            //"status_id":"1",
            //"total":"190",
            //"order_items":[{"product_id":"2","name":"Salmon Roll","price":"18","quantity":"4"},{"product_id":"3","quantity":"3"},{"product_id":"4","quantity":"2"}]
            //}
          ]
    }
  },
  mounted() {
    this.getOrders();
  },
  methods: {
    async getOrders() {
      if (await this.$store.dispatch('orders/getMyOrdersFromDB')) {
        this.orders = this.$store.getters['orders/getOrders'];
      }
    },
    getUser() {
      this.user = this.$store.getters['user/getUser']
    }
  },
  computed: {},
}
</script>

<style scoped>
</style>