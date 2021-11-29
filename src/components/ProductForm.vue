<template>
 
    <form class="product-form">
      <div class="input-container">
        <label for="product-name" class="required-label">Наименование товара</label>
        <input v-model="product.name" @focus="checkFocus" @blur="inputFocused = ''" autocomplete="off" class="form__input" name ="name" type="text" id="product-name" placeholder="Введите наименование товара">
        <div v-show="inputFocused === 'name'" class="required-message" >Это обязательное поле</div>
      </div>

      <label for="product-description">Описание товара</label>
      <textarea v-model="product.description" class="form__input" name ="description" id="product-description" placeholder="Введите описание товара"></textarea>

      <div class="input-container">
        <label for="product-image" class="required-label">Ссылка на изображение товара</label>
        <input v-model="product.image" @focus="checkFocus" @blur="inputFocused = ''" autocomplete="off" class="form__input" type="text" name="image" id="product-image" placeholder="Введите ссылку">
        <div v-show="inputFocused === 'image'" class="required-message" >Это обязательное поле</div>
      </div>
      
      <div class="input-container">
        <label for="product-price" class="required-label">Цена товара</label>
        <input v-model="product.price" @focus="checkFocus" @input="regExpres"  @blur="inputFocused = ''"  autocomplete="off" class="form__input" type="text" name="price" id="product-price" placeholder="Введите цену">
        <div v-show="inputFocused === 'price'" class="required-message" >Это обязательное поле</div>
      </div>
    
      <button @click.prevent="addProduct" :disabled="!checkForm">Добавить товар</button>
    </form>
</template>
<script>
export default {
  data() {
    return {
      product: {
        name:'',
        description: '',
        image: '',
        price: '',
      },
      inputFocused: '',        
    }
  },

  computed: {
    checkForm: function () {
      if (this.product.name && this.product.image && this.product.price) {
        return true;
      }
      return false;
    }
  },

  methods: {
    addProduct(){
        this.$emit('addProduct', this.product)
        this.product = {
          name:'',
          description: '',
          image: '',
          price: '' 
        }
    },

    checkFocus(e) {
      this.inputFocused = e.target.name
    },
    regExpres(e) {
      e.target.value = e.target.value.replace(/[^0-9]/g, '');
      e.target.value = e.target.value.replace(/(\d)(?=(\d\d\d)+([^\d]|$))/g, '$1 ');
      console.log(e.target.value)
    }
  },
}
</script>
<style lang="scss" scoped>
  @mixin shadow-border {
    box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
    border: none;
    border-radius: 4px;
  }
  
  $decor-color:#FF8484;

  .product-form {
    display: flex;
    flex-direction: column;
    width: calc(25% - 8px);
    min-width: 245px;
    max-height: 460px;
    padding: 24px;
    margin-right: 16px;
    background: #FFFEFB;
    @include shadow-border;
  }

  input, textarea {
    @include shadow-border;
    width: 100%;
    padding-left: 16px;
    padding-top: 10px;
    padding-bottom: 11px;
    color: #49485E;
    font-size: 12px;
    line-height: 1.26;
      &:last-child {
        margin-bottom: 0;
      }
      &:focus-visible{
        outline: none;
      }
  }
  
  textarea:focus {
    outline: 1px solid #ff6e1b;
  }
  input:focus {
    outline: 1px solid $decor-color;
  }
  

  textarea {
    min-height: 108px;
    margin-bottom: 16px;
    resize: none;
  }

  label {
    position: relative;
    margin-bottom: 4px;
    font-size: 10px;
    &.required-label::after {
      content:'';
        position: absolute;
        width: 4px;
        height: 4px;
        border-radius: 4px;
        background-color: $decor-color;  
    
    }
  }

  .input-container {
    position: relative;
    width: 100%;
    margin-bottom: 16px;
  }

  .required-message {
    position: absolute;
    color: $decor-color;
    font-size: 8px;
  }
  button {
    min-height: 36px;
    background-color: #7BAE73;
    color: #fff;
    border: none;
    box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
    border-radius: 10px;
    opacity: 1;
    transition: opacity 0.3s ease;
    &:disabled {
      color: #B4B4B4;
      background: #EEEEEE;
    }
    &:hover {
      opacity: 0.9;
    }
  }

  @media (max-width:865px) {
    .product-form {
      margin-left: auto;
      margin-right: auto;
      margin-bottom: 16px;
      width: 70%;

    }
    
  }
  
</style>