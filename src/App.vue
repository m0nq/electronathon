<template>
  <div id="app" class="container mt-5">
    <router-view
      :cart="cart"
      :cartQty="cartQty"
      :cartTotal="cartTotal"
      :sliderStatus="sliderStatus"
      :maximum.sync="maximum"
      :products="products"
      @add="addItem"
      @delete="deleteItem"
      @toggle="toggleSliderStatus"
    ></router-view>
  </div>
</template>

<script>
  export default {
    name: 'app',
    data() {
      return {
        maximum: 99,
        sliderStatus: true,
        cart: [],
        products: null
      };
    },
    computed: {
      cartTotal() {
        let sum = 0;
        for (const key in this.cart) {
          sum += this.cart[key].product.price * this.cart[key].qty;
        }
        return sum;
      },
      cartQty() {
        let qty = 0;
        for (const key in this.cart) {
          qty += this.cart[key].qty;
        }
        return qty;
      }
    },
    methods: {
      toggleSliderStatus() {
        this.sliderStatus = !this.sliderStatus;
      },
      deleteItem(id) {
        if (this.cart[id].qty > 1) {
          this.cart[id].qty--;
        } else {
          this.cart.splice(id, 1);
        }
      },
      addItem(product) {
        let whichProduct;
        const existing = this.cart.filter((item, index) => {
          if (item.product.id === Number(product.id)) {
            whichProduct = index;
            return true;
          }
          return false;
        });

        if (existing.length) {
          this.cart[whichProduct].qty++;
        } else {
          this.cart.push({
            product: product,
            qty: 1
          });
        }
      }
    },
    mounted() {
      fetch('https://hplussport.com/api/products/order/price')
        .then(response => response.json())
        .then(data => {
          this.products = data;
        });
    }
  };
</script>
