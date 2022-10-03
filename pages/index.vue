<template>
  <section>
    <NavBarComponent></NavBarComponent>
    <div class="container">
      <div class="d-flex justify-content-center">
        <div class="d-flex flex-column w-25">
          <label for="category">Chose Category:</label>
          <select class="p-2 mb-3 text-center" id="category" aria-label="Select" name="category"
                  v-model="category_selected">
            <option v-for="category in categories" :value="category.id">{{ category.name }}</option>
          </select>
        </div>
      </div>
      <div class="row">
        <div v-for="product in products.data" class="mb-4 col-12 col-sm-12 col-md-4 ">
          <div class="py-3 text-center border border-secondary rounded">
            <h4>{{ product.name }}</h4>
            <h5>{{ product.price }}$</h5>
            <p>{{ product.description }}</p>
            <nuxt-link
              :to="{ name: 'product', query: { name: product.name, price:product.price, description: product.description }}">
              See Product
            </nuxt-link>
          </div>
        </div>
      </div>
    </div>
  </section>

</template>

<script>
import axios from 'axios'
import NavBarComponent from "~/components/NavBarComponent";

export default {
  name: 'IndexPage',
  components: {'NavBarComponent': NavBarComponent},
  data() {
    return {
      categories: [],
      category_selected: 1,
      products: []
    }
  },
  async mounted() {
    this.categories = await axios.get('http://127.0.0.1:8000/api/categories')
      .then(function (res) {
        return res.data.data
      })
    this.products = await axios.get('http://127.0.0.1:8000/api/products/1')
      .then(function (res) {
        return res.data
      })
  },
  watch: {
    category_selected: async function () {
      this.products = await axios.get(`http://127.0.0.1:8000/api/products/${this.category_selected}`)
        .then(function (res) {
          return res.data
        })
    }
  },
  methods: {
    productLink(id) {
      return this.$router.push({path: 'register', query: {plan: 'private'}})
    }
  }
}
</script>

<style scoped>
.bg-custom {
  background-color: deepskyblue;
}

a:hover {
  text-decoration: none;
}
</style>
