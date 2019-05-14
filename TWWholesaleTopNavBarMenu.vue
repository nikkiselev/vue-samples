<script>
  import WholesaleCart from '../models/WholesaleCart';

  export default {
    props: {
      user: {
        type: [Object],
        required: false,
        default() {
          return {};
        }
      }
    },

    data() {
      return {
        /**
         * @type {string}
         */
        searchQuery: '',

        /**
         * @type {[Object]}
         */
        cartItems: []
      };
    },

    computed: {

      /**
       * Total cost price of items in the cart.
       *
       * @returns {number}
       */
      totalCostPrice() {
        let total = 0.0;
        this.cartItems.forEach(item => total += item.pivot.quantity * item.wholesale_price);
        return _.round(total);
      },

      /**
       * @returns {number}
       */
      numberOfItemsInCart = () => this.cartItems.length,

      /**
       * Determine is user authenticated.
       *
       * @returns {boolean}
       */
      authenticated = () => this.user.id !== undefined,
    },

    methods: {
      fetchCart() {
        WholesaleCart.all().then(({data}) => {
          this.cartItems = data.data;
        }).catch(e => api.handleErrors(e));
      },

      search() {
        window.location.href = window.location.origin + '?q=' + this.searchQuery;
      },

      logout() {
        api.post('/logout').then(() => window.location.href = '/').catch(e => api.handleErrors(e));
      }
    },

    created() {
      if (this.authenticated) {
        this.fetchCart();
        eventHub.$on('cartWasFetched', cartItems => this.cartItems = cartItems);
      }
    }

  }
</script>

<template>
  <nav class="navbar navbar-expand-lg fixed-top border-bottom wholesale-navbar">
    <button
      class="navbar-toggler"
      type="button"
      data-toggle="collapse"
      data-target="#navbarCollapse"
      aria-controls="navbarCollapse"
      aria-expanded="false"
      aria-label="Toggle navigation"
    >
      <span class="navbar-toggler-icon"></span>
    </button>
    <div
      class="collapse navbar-collapse"
      id="navbarCollapse"
    >
      <ul class="navbar-nav ml-auto">
        <li
          v-if="authenticated"
          class="nav-item"
        >
          <a
            class="nav-link text-dark"
            href="/cart"
          >
            <i class="fas fa-shopping-cart"></i> Cart
            <span>  ({{ numberOfItemsInCart }})  ${{ totalCostPrice }}</span>
          </a>
        </li>
        <li
          v-if="authenticated"
          class="nav-item"
        >
          <a
            class="nav-link text-dark"
            href="/account"
          >
            <i class="fas fa-user"></i> Past orders
          </a>
        </li>
        <li
          v-if="authenticated"
          class="nav-item"
        >
          <a
            @click.prevent="logout"
            class="nav-link text-dark"
            href="#"
          >
            <i class="fas fa-sign-in-alt"></i> Logout
          </a>
        </li>
        <li
          v-else
          class="nav-item"
        >
          <a
            class="nav-link"
            href="/login"
          > <i class="fas fa-sign-in-alt"></i> Login
          </a>
        </li>
      </ul>
      <div class="form-inline mt-2 mt-md-0">
        <div class="input-group input-group-sm">
          <input
            v-model="searchQuery"
            @keyup.enter="search"
            class="form-control mr-xs-2"
            placeholder="Search"
            aria-label="Search"
          >
        </div>
      </div>
    </div>
  </nav>
</template>
<style>

  .wholesale-navbar {
    font-family: "Open Sans", "HelveticaNeue", "Helvetica Neue", sans-serif;
    font-size: 0.875em;
    font-weight: 400;
    background-color: white;
  }

  .dropdown-submenu {
    position: relative;
  }

  .dropdown-submenu > .dropdown-menu {
    top: 0;
    left: 100%;
    margin-top: -6px;
    margin-left: -1px;
    -webkit-border-radius: 0 6px 6px 6px;
    -moz-border-radius: 0 6px 6px;
    border-radius: 0 6px 6px 6px;
  }

  .dropdown-submenu:hover > .dropdown-menu {
    display: block;
  }

  .dropdown-submenu > a:after {
    display: block;
    content: " ";
    float: right;
    width: 0;
    height: 0;
    border-color: transparent;
    border-style: solid;
    border-width: 5px 0 5px 5px;
    border-left-color: #ccc;
    margin-top: 5px;
    margin-right: -10px;
  }

  .dropdown-submenu:hover > a:after {
    border-left-color: #fff;
  }

  .dropdown-submenu.pull-left {
    float: none;
  }

  .dropdown-submenu.pull-left > .dropdown-menu {
    left: -100%;
    margin-left: 10px;
    -webkit-border-radius: 6px 0 6px 6px;
    -moz-border-radius: 6px 0 6px 6px;
    border-radius: 6px 0 6px 6px;
  }
</style>
