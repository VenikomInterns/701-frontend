<template>
  <section>
    <NavBarComponent></NavBarComponent>
    <div class="container">
      <div class="py-3 text-center border border-secondary rounded">
        <h4>Name:</h4>
        <h4 class="mb-3">{{ name }}</h4>
        <h4>Price</h4>
        <h5 class="mb-3">{{ price }}$</h5>
        <h4>Description</h4>
        <p>{{ description }}</p>
      </div>
    </div>

  </section>
</template>

<script>
import NavBarComponent from "~/components/NavBarComponent";
export default {
  name: "product",
  components: {'NavBarComponent': NavBarComponent},
  data() {
    return {
      name: this.$route.query.name,
      price: this.$route.query.price,
      description: this.$route.query.description
    }
  },
}
</script>

<style scoped>

</style>
