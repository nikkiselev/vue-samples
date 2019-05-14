<script>
  import TWWholesaleCartItem from './TWWholesaleCartItem.vue';
  import WholesaleCart from '../models/WholesaleCart';

  export default {
    components: {'tw-wholesale-cart-item': TWWholesaleCartItem},

    data() {
      return {
        /**
         * @type {[Object]}
         */
        items: []
      };
    },

    computed: {

      /**
       * @returns {string}
       */
      totalCost() {
        let cost = 0.0;
        this.items.forEach(({wholesale_price, pivot}) => cost += wholesale_price * pivot.quantity);
        return cost.toFixed(2);
      }
    },

    methods: {
      fetchCartItems() {
        WholesaleCart.all().then(({data}) => this.items = data.data).catch(e => api.handleErrors(e));
      },

      /**
       * @param {int} index
       */
      removeItem(index) {
        this.items.splice(index, 1);
      },

      checkout() {
        window.location.href = '/checkout';
      }
    },

    created() {
      this.fetchCartItems();
    },
  }
</script>

<template>
  <div class="row mt-5">
    <div class="col">
      <h1 class="text-secondary">Cart</h1>
      <hr>
      <table class="table cart">
        <tbody>
        <tw-wholesale-cart-item
          v-for="(item, index) in items"
          :data="item"
          :key="item.id"
          @itemWasRemoved="removeItem(index)"
        />
        </tbody>
      </table>
      <hr>
      <div class="text-right">
        <span class="font-weight-bold pr-3">Total ${{ totalCost }}</span>
      </div>
      <hr>
      <div class="text-right">
        <button
          @click="checkout"
          class="btn btn-action"
          type="button"
        >Checkout
        </button>
      </div>
    </div>
  </div>
</template>

<style>
  .cart td {
    border-top: none;
    vertical-align: middle;
  }
</style>