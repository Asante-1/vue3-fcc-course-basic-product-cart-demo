<template>
  <header class="top-bar spread">
        <nav class="top-bar-nav">
          <router-link to="/" class="top-bar-link">
            <i class="icofont-spoon-and-fork"></i>
            <span>Home</span>
          </router-link>
          <router-link to="/products" class="top-bar-link">
            <span>Products</span>
          </router-link>
          <router-link to="past-orders" class="top-bar-link">
            <span>Past Orders</span>
          </router-link>
        </nav>
        <div @click="toggleSideBar" class="top-bar-cart-link">
          <i class="icofont-cart-alt icofont-1x"></i>
          <span>Cart ({{ totalQuantity }})</span>
        </div>
      </header>
  <router-view :inventory="inventory" :addToCart="addToCart" />

  <SideBar
      v-if="showSideBar"
      :toggle="toggleSideBar"
      :cart= "cart"
      :inventory="inventory"
      :remove="deleteItem"
  />

</template>

<script>
import SideBar from '@/components/SideBar.vue'
import food from './food.json'

export default {
  components: {
    SideBar
  },

  data () {
    return {
      showSideBar: false,
      inventory: food,
      cart: {}
    }
  },

  computed: {
    totalQuantity () {
      return Object.values(this.cart).reduce((acc, curr) => {
        return acc + curr
      }, 0)
    }
  },

  methods: {
    addToCart (name, index, quantity) {
      if (!this.cart[name]) this.cart[name] = 0
      this.cart[name] += quantity
    },
    toggleSideBar () {
      this.showSideBar = !this.showSideBar
    },
    deleteItem (name) {
      delete this.cart[name]
    }
  }
}
</script>
