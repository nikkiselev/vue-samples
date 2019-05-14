<script>
  export default {
    props: {

      meta: {
        type: Object,
        required: true,
      },

    },

    data() {
      return {

        /** @type {int} */
        page: this.meta.current_page,

      };
    },

    computed: {
      hasPrevPage = () => this.meta.current_page > 1,

      hasNextPage = () => this.meta.current_page < this.meta.last_page,

      prevPage = () => this.meta.current_page - 1,

      nextPage = () => this.meta.current_page + 1,
    },

    methods: {
      decrementPage() {
        if (this.page > 1) {
          this.page--;
        }
      },

      incrementPage() {
        if (this.hasNextPage) {
          this.page++;
        }
      }
    },

    watch: {

      'meta.current_page'(page) {
        this.page = page;
      },

      page(page) {
        this.$emit('updated', page);
      },

    }

  }
</script>

<template>
  <div>
    <ul class="pagination">
      <li
        :class="{disabled: !hasPrevPage}"
        class="paginate_button previous"
      >
        <a
          @click.prevent="decrementPage"
          href="#"
        >Previous</a>
      </li>

      <li
        v-show="hasPrevPage"
        class="paginate_button"
      >
        <a
          @click.prevent="page = prevPage"
          href="#"
        >{{ prevPage }}</a>
      </li>
      <li class="paginate_button active">
        <a href="#">{{ meta.current_page }}</a>
      </li>
      <li
        v-show="hasNextPage"
        class="paginate_button"
      >
        <a
          @click.prevent="page = nextPage"
          href="#"
        >{{ nextPage }}</a>
      </li>

      <li
        :class="{disabled: !hasNextPage}"
        class="paginate_button next"
      >
        <a
          @click.prevent="incrementPage"
          href="#"
        >Next</a>
      </li>
    </ul>
  </div>
</template>
