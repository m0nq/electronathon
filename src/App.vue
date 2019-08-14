<template>
  <div id="app" class="container mt-5">
    <img src="images/binaryville.svg" style="width: 300px;" alt="Binaryville Logo"
         class="img-fluid m-auto d-block">
    <router-view
      :cart="cart"
      :cartQty="cartQty"
      :cartTotal="cartTotal"
      :products="products"
      @add="addItem"
      @delete="deleteItem"
      @setCurrentPhoto="setCurrentPhoto"
    ></router-view>
  </div>
</template>

<script>
  export default {
    name: 'app',
    data() {
      return {
        cart: [],
        products: null,
        modalPhoto: null
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
      setCurrentPhoto(item) {
        this.modalPhoto = item.replace('_tn', '');
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
