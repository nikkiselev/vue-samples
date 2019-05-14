<script>
  import BinLocationsAddItem from './BinLocationsAddItem.vue';
  import BinLocationsItem from './BinLocationsItem.vue';
  import TWTable from './TWTable.vue';

  export default {

    components: {
      BinLocationsAddItem,
      BinLocationsItem,
      TWTable,
    },

    data() {
      return {
        /**
         * Table columns.
         */
        columns: [
          {
            name: 'ID',
            sortable: false,
          },
          {
            name: 'Code',
            sortable: false,
          },
          {
            name: 'Actions',
            sortable: false,
          },
        ],
      }
    },

    computed: {

      /**
       * Vue component used as table row.
       *
       * @returns {object}
       */
      itemComponent = () => BinLocationsItem,
    },

    methods: {

      /**
       * Add item to the list.
       *
       * @param {Object} item
       */
      add(item) {
        eventHub.$emit('table-add-item', item);
      },

    },

  }
</script>

<template>
  <div>
    <bin-locations-add-item @added="add"/>

    <div class="row">
      <div class="col-md-12">
        <div class="ibox">
          <div class="ibox-content">
            <t-w-table
              endpoint="/api/bin-locations"
              :columns="columns"
              :item-component="itemComponent"
              :paginate="false"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
