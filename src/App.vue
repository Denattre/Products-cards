<template>
  <div class="app">
    <header>
      <h1>Добавление товара</h1>
      <SortSelect @changeOrder = "sortProducts" :productsArray="productsArray"></SortSelect>
    </header>
    <div class="content">
      <ProductForm @addProduct="createCard"></ProductForm>
      <div v-if="isLoading">Загрузка списка товаров</div>
      <transition-group v-else tag="ul" name="list" class="cards-container">
        <Card v-for="(product, index) in productsArray" :key="product.id" class="card" :product="product" @deleteCard="deleteProduct(index)"></Card>
      </transition-group>
    </div>
  </div>
</template>

<script>
import ProductForm from "@/components/ProductForm.vue"
import Card from "@/components/Card.vue"
import SortSelect from '@/components/SortSelect.vue'


export default {
  components: {
    ProductForm,
    Card,
    SortSelect
  },
  data() {
    return {
      product: {},
      productsArray: [],
      id: 1,
      isLoading: false
    }
  },

  mounted() {
    if (localStorage.getItem("productsArray") !== null) {
      this.isLoading = true;
      this.productsArray = JSON.parse(localStorage.getItem("productsArray"));
      this.isLoading = false;
    } 
  },

  watch: {
    productsArray: function(newProductsArray) {
      localStorage.setItem('productsArray', JSON.stringify(newProductsArray))
    },
  },

  methods: {
    createCard(product) {
      product.price = product.price.replace(/[^0-9]/g, '').replace(/(\d)(?=(\d\d\d)+([^\d]|$))/g, '$1 ')
      product.id = this.id
      this.id++
      this.productsArray.push(product)
      localStorage.setItem('productsArray', JSON.stringify(this.productsArray))
    },
    deleteProduct(index) {
      this.productsArray.splice(index, 1)
      localStorage.setItem('productsArray', JSON.stringify(this.productsArray))
    },
    sortProducts(sortType) {
      if (sortType == "asc") {
        this.productsArray.sort(function(a, b) {
          return a.price.replace(/[^0-9]/g, '') - b.price.replace(/[^0-9]/g, '');
        });
      } else if (sortType == "desc") {
        this.productsArray.sort(function(a, b) {
          return b.price.replace(/[^0-9]/g, '') - a.price.replace(/[^0-9]/g, '');
        });
      } else {
        this.productsArray.sort(function(a, b){
            let nameA=a.name.toLowerCase(), nameB=b.name.toLowerCase()
            if (nameA < nameB) return -1
            if (nameA > nameB) return 1
            return 0 
          })
      }
      localStorage.setItem('productsArray', JSON.stringify(this.productsArray))
    }
  },
}
</script>

<style lang="scss">

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
    padding-bottom: 20px;
  }

  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
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
    padding: 0;
    gap: 16px;
    width: 75%;
  }

  .card {
      width: calc(33% - 9px);
      list-style: none;
    }
  .list-move {
    transition: transform 1s;
  }
  .list-enter-active, .list-leave-active {
    transition: all 1s;
  }
  .list-enter-from, .list-leave-to {
    opacity: 0;
    transform: translateY(30px);
  }

  @media (max-width:865px) {

    header {
      flex-direction: column;
      margin-bottom: 16px;
    }
    .content {
      flex-direction: column;
    }
   
    .cards-container {
      width: 100%;
    }
    
    .card {
      width: calc(50% - 16px);
    }
  }
  
</style>
