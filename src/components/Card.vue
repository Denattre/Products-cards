<template>
<li @mouseover="cardHover = true" @mouseout="cardHover = false" class="product-card">
  <div class="card-content">
    <img :src="product.image" :alt="product.name">
    <h2>{{product.name}}</h2>
    <p>{{product.description}}</p>
    <p class="product-price">{{product.price}} руб.</p>
  </div>
    <button @click="deleteCard" v-show="cardHover" class="delete"></button>
</li>
  
</template>
<script>
export default {
  props: {
    product: {
      type: Object,
    }
  },
  data() {
    return {
      cardHover: false
    }
  },
  methods: {
    deleteCard() {
      this.$emit('deleteCard')
    }
  },
  
}
</script>
<style lang="scss" scoped>
  p {
    margin: 0;
  }
  $range:16px;

  .product-card {
    position: relative;
  }
  .card-content {
    display: flex;
    flex-direction: column;
    min-height: 423px;
    height: 100%;
    padding: 0 $range 24px $range;
    background: #fff;
    box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02);
    border-radius: 4px;
    overflow: hidden;
    cursor: pointer;
  }
  
  img {
    margin-left:-$range;
    margin-bottom: $range;
    width: calc(100% + 2*$range);
    height: 12rem;
    object-fit: cover;
  }

  h2 {
    margin-top: 0;
    margin-bottom: $range;
    font-size: 20px;
    font-weight: 600;
  }
  
  .product-price {
    font-size: 24px;
    font-weight: 600;
    margin-top: auto;
  }

  .delete {
    position: absolute;
    left: 94%;
    bottom: 96%;
    width: 32px;
    height: 32px;
    background-color: transparent;
    border: none;
    background-size: cover;
    background-image: url('../assets/images/delete-icon.svg');
    background-repeat: no-repeat;
    cursor: pointer;
  }
</style>