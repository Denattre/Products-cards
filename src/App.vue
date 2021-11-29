<template>
  <div class="app">
    <h1>Добавление товара</h1>
    <div class="content">
      <ProductForm @addProduct="createCard"></ProductForm>
      <ul class="cards-container">
        <li v-for="product in productsArray" :key="product.id" class="card">
          <Card :product="product" @deleteCard="deleteProduct"></Card>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import ProductForm from "@/components/ProductForm.vue"
import Card from "@/components/Card.vue"
export default {
  components: {
    ProductForm,
    Card
  },
  data() {
    return {
      product: {
        // name:'Наименование товара',
        // description: "Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строк",
        // image: './assets/images/product-image.jpg',
        // price: "10 000 руб."
      },
      productsArray: [],
      id: 1,
    }
  },
  // mounted() {
  //   if (localStorage.productsArray) {
  //     this.productsArray = localStorage.productsArray;
  //   }
  // },
  watch: {
    productsArray: function(newProductsArray) {
      localStorage.productsArray = newProductsArray;
      // console.log(newProductsArray)
    },
  },
  methods: {
    createCard(product) {
      product.price = product.price.replace(/(\d)(?=(\d\d\d)+([^\d]|$))/g, '$1 ')
      product.id = this.id
      this.id++
      this.productsArray.push(product)
    },
    deleteProduct(idToDelete) {
      console.log(this.productsArray)
      this.productsArray.forEach(function(el, i) {
        if (el.id == idToDelete) {
          this.productsArray.splice(i, 1)
        } 
        
      })
      console.log("Удалено")
    }
  },
}
</script>

<style >

  @font-face {
    font-family: 'sourceSansPro';
    src: url('./assets/fonts/sourcesanspro-regular.woff2') format('woff2'),
         url('./assets/fonts/sourcesanspro-regular.woff') format('woff');
    font-weight: 400;
    font-style: normal;
  }

  @font-face {
      font-family: 'sourceSansPro';
      src: url('./assets/fonts/sourcesanspro-semibold.woff2') format('woff2'),
          url('./assets/fonts/sourcesanspro-semibold.woff') format('woff');
      font-weight: 600;
      font-style: normal;
  }
  
  * {
    box-sizing: border-box;
  }
  html {
    font-family: 'sourceSansPro';
    font-weight: 400;
    font-size: 16px;
    background: #E5E5E5;
  }

  body{
    margin: 0;
  }
  
  .app {
    margin-left: 32px;
    margin-right: 32px;
  }

  h1 {
    font-size: 28px;
    font-weight: 600;
  }
  
  ul {
    margin: 0;
  }
  .content{
    display: flex;
    width: 100%;
  }

  .cards-container {
    display: flex;
    flex-wrap: wrap;
    gap: 16px;
    width: calc(66% - 16px);
  }

  .card {
      width: calc(33% - 16px);
      list-style: none;
    }

  
</style>
