<template>
  <section>
    <NavBarComponent></NavBarComponent>
    <div class="container">
      <div class="d-flex justify-content-center">
        <div class="d-flex flex-column w-25">
          <label for="category">Chose Category:</label>
          <select class="p-2 mb-3 text-center" id="category" aria-label="Select" name="category"
                  v-model="category_selected">
                  <!--Misssing :key="" attribute-->
            <option v-for="category in categories" :value="category.id">{{ category.name }}</option>
          </select>
        </div>
      </div>
      <div class="row">
        <!--initially products is array so this will result in console log errors-->
        <div v-for="product in products.data" class="mb-4 col-12 col-sm-12 col-md-4 ">
          <div class="py-3 text-center border border-secondary rounded">
            <h4>{{ product.name }}</h4>
            <h5>{{ product.price }}$</h5>
            <p>{{ product.description }}</p>
            <nuxt-link
              :to="{ name: 'product', query: { name: product.name, price:product.price, description: product.description }}">
              See Product
            </nuxt-link>
            <!--sending product informations through url query ? What if we have description long thousands chars-->
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
      products: [] //initialy is array but after api call finishes ->  will be object.
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
      }) // you should make this reusable
  },
  watch: {
    category_selected: async function () {
      this.products = await axios.get(`http://127.0.0.1:8000/api/products/${this.category_selected}`)
        .then(function (res) {
          return res.data
        }) // if u use await  then u dont need .then. or if u use .then u dont need await.
    }
  },
  methods: { //defined but not used
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
