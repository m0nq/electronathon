<template>
  <div>
    <navbar :cart="cart" :cartQty="cartQty" :cartTotal="cartTotal"></navbar>
    <h2 class="text-center mt-0">My Shop</h2>
    <p>Share your love of technology, and spread good cheer, with Binaryville official...</p>
    <product-list :products="products"></product-list>
  </div>
</template>
<script>
  import Navbar from './Navbar.vue';
  import ProductList from './ProductList.vue';

  export default {
    name: 'products',
    props: [
      'products',
      'maximum',
      'cart',
      'cartQty',
      'cartTotal'
    ],
    components: {
      Navbar,
      ProductList
    }
  };
</script>
