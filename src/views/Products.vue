<template>
  <div>
    <Header/>
    <br><br><br>
    <div v-for="product in products" :key="product.id">
      <div class="wrapper">
        <div class="container">
          <div class="row">
              <div class="blogPost">
                  <div class="card-body col-4">
                    <h4><strong>{{ product.name }}</strong></h4>
                  </div>
                  <img :src="'http://localhost/LAB11/public/images/' + product.image" width="10%" height="10%" alt=""/></div>

            <h1>{{product.price}}$</h1>
            <p style="float:left;"><button @click="addToBasket(product.id)" class="btn btn-warning">Add To Basket</button></p>

          </div>
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
  showModal: true,
  components: {
    Footer,
    Header
  },
  data() {
    return {
      products: [],
    }
  },
  mounted() {
    this.getProducts();
  },
  methods: {
    async getProducts() {
      if (await this.$store.dispatch('products/getProductsFromDB')) {
        this.products = this.$store.getters['products/getProducts']
      }
    },
    addToBasket(productID){
      this.$store.commit('basket/incrementProduct',productID)
    }
  },
  computed: {},
}


</script>

<style scoped>
</style>