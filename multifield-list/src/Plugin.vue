<template>
  <div class="List">
    <ol class="List__list uk-margin-bottom-remove">
      <li
        v-for="(item, index) in model.items"
        :key="index"
        class="List__list-item uk-flex uk-flex-middle"
      >
        <div>
          <label>{{ options.field_1_name }}</label>
          <input
            v-if="hasField1"
            v-model="model.items[index][options.field_1_name]"
            class="uk-form-small uk-width-1-1"
            :aria-label="`List item ${index}`"
          >
        </div>
        <div>
          <label>{{ options.field_2_name }}</label>
          <input
            v-if="hasField2"
            v-model="model.items[index][options.field_2_name]"
            class="uk-form-small uk-width-1-1"
            :aria-label="`List item ${index}`"
          >
        </div>
        <div>
          <label>{{ options.field_3_name }}</label>
          <input
            v-if="hasField3"
            v-model="model.items[index][options.field_2_name]"
            class="uk-form-small uk-width-1-1"
            :aria-label="`List item ${index}`"
          >
        </div>
        <a
          class="assets__item-trash"
          aria-label="Remove item"
          @click="removeItem(index)"
        >
          <i class="uk-icon-minus-circle"></i>
        </a>
      </li>
    </ol>
    <a
      class="blok__full-btn uk-margin-small-top"
      @click="addItem"
    >
      <i class="uk-icon-plus-circle uk-margin-small-right"/>
      Add item
    </a>
  </div>
</template>

<script>
export default {
  mixins: [window.Storyblok.plugin],
  computed: {
    hasAtLeastOneFields() {
      return this.hasField1 || this.hasField2 || this.hasField3;
    },
    hasField1() {
      return this.options.field_1_name != "";
    },
    hasField2() {
      return this.options.field_2_name != "";
    },
    hasField3() {
      return this.options.field_3_name != "";
    },
  },
  methods: {
    initWith() {
      return {
        // needs to be equal to your storyblok plugin name
        plugin: 'multifield-list',
        items: []
      }
    },
    pluginCreated() {
      // eslint-disable-next-line
      console.log('View source and customize: https://github.com/storyblok/storyblok-fieldtype')
    },
    addItem() {
      if(!this.hasAtLeastOneFields)
        return
      
      this.model.items.push({
        [this.options.field_1_name]: (this.hasField1? "" : undefined),
        [this.options.field_2_name]: (this.hasField2? "" : undefined),
        [this.options.field_3_name]: (this.hasField3? "" : undefined),
      });
    },
    removeItem(index) {
      this.model.items = this.model.items.filter((_, i) => i !== index)
    },
  },
  watch: {
    'model': {
      handler: function (value) {
        this.$emit('changed-model', value);
      },
      deep: true
    }
  }
}
</script>

<style>
  .p-metatags__google-title {
    color: blue;
    text-decoration: underline;
  }

  .p-metatags__google-link {
    color: green;
  }

  .p-metatags__preview {
    margin: 5px 0 15px;
    padding: 10px;
    color: #000;
    background: #FFF;
  }
  .List__list {
    padding-left: 0;
  }
  
  .List__list-item + .List__list-item {
    margin-top: 5px;
  }
</style>
