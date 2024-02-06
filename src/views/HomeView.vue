<template>
  <div class="filterWrapper">
    <div class="topFlex">
      <div
      :class="activeCategory===item?'activeCategory':''" 
      @click="activeCategory=activeCategory===item ? '':item" 
      v-for="item in categories" 
      class="category">
        {{ item }}
      </div>
    </div>
    <div class="box">
      <div class="badge">
        {{ choseProduct.length }}
      </div>
      <svg @click="onShow" viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><path d="M6 2L3 6v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2V6l-3-4z"></path><line x1="3" y1="6" x2="21" y2="6"></line><path d="M16 10a4 4 0 0 1-8 0"></path></svg>
      <ShopContext v-if="show" @on-go-basket="show=false" :item="choseProduct"/>
    </div>
  </div>
  <div class="bottomFlex">
    <ProductCard @on-item-box="onItemFunc($event)" v-for="item in products" :item="item"/>
  </div>
</template>

<script setup>
import ShopContext from '@/components/ShopContext.vue';
import ProductCard from '@/components/ProductCard.vue';
import {ref,onMounted,watch} from "vue"

const show = ref(false)
const categories = ref([])
const products = ref([])
const activeCategory = ref(null)
const choseProduct = ref([])

const onShow = ()=>{
  show.value = !show.value
}

const onItemFunc = (e)=>{
  choseProduct.value.push(e)
}

const fetchCategories = async()=>{
  try {
    const response = await fetch("https://fakestoreapi.com/products/categories")
    const jsonData = await response.json()
    categories.value = jsonData
    console.log(categories.value)
  } catch (error) {
    console.log(error)
  }
}

const fetchProducts = async()=>{
  try {
    const response = await fetch("https://fakestoreapi.com/products")
    const jsonData = await response.json()
    products.value = jsonData
    console.log(products.value)
  } catch (error) {
    console.log(error)
  }
}

const fetchProductsByCategory = async(category)=>{
  try {
    const response = await fetch(`https://fakestoreapi.com/products/category/${category}`)
    const jsonData = await response.json()
    products.value = jsonData
    console.log(products.value)
  } catch (error) {
    console.log(error)
  }
}

watch(activeCategory,(newVal)=>{
  if(newVal === ""){
    fetchProducts()
  }else{
    fetchProductsByCategory(newVal)
  }
})

onMounted(() => {
  fetchCategories()
})

onMounted(()=>{
  fetchProducts()
})

</script>

<style scoped>
.filterWrapper{
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 20px;
  flex-wrap: wrap;
}

.topFlex{
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 20px;
}

.category{
  cursor: pointer;
  padding: 4px 16px;
  background-color: #fafafa;
  border: 1px solid grey;
  font-size: 12px;
  font-weight: 500;
  border-radius: 16px;
  margin-top: 8px;
}

.activeCategory{
  border: 1px solid red;
  background: red;
  color:white;
  font-weight: 600;
}

.box{
  position: relative;
  cursor: pointer;
  color: #374051;
  transition:all 0.5s;
}

.box:hover{
  color:#1874ff;
}

.badge{
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top:-8px;
  right: -8px;
  font-size: 10px;
  background-color: #1874ff;
  border-radius: 100%;
  color:white;
  width: 16px;
  height: 16px;
}

.bottomFlex{
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 20px;
  flex-wrap: wrap;
  margin-top: 16px;
}
</style>