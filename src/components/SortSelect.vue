<template>
  <div class="select-block">
      <select @change="changeOrder" class="sort-select" :disabled="productsArray.length == 0">
        <option value="defauld" selected>По умолчанию</option>
        <option value="asc">По возрастанию</option>
        <option value="desc">По убыванию</option>
      </select>
  </div>
</template>
<script>
export default {
  props: {
    productsArray: {
      type: Object
    }
  },

  methods: {
    changeOrder(event) {
      this.$emit('changeOrder', event.target.value)
    }
  },
}
</script>
<style lang="scss" scoped>
@mixin user-accessibility {
  border: none;
  border-radius: 2px;
  box-shadow: 0 0 0 1pt #ff6e1b; 
}
.select-block {
  width: 122px;
  height: 36px;   
}
  .sort-select {
    height: 100%;
    width: 100%;
    padding: 10px 0px 11px 9px;
    color: #B4B4B4;
    border: none;
    border-radius: 4px;
    background: #FFFEFB;
    box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
    font-size: 12px;
    &:hover {
      @include user-accessibility();
    }
    &:disabled {
      box-shadow: none;
    }
  }
  
</style>