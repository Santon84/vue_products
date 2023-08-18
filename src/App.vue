

<script setup>
/* eslint-disable */
import { ref, onMounted, computed, watch, transformVNodeArgs} from 'vue'
const itemList = ref([]);
const name = ref('');
const input_content = ref('');

const sorted_productsList = computed(() => {
  
    return itemList.value
    .sort((a,b) => a.createdAt - b.createdAt)
    .sort((a,b) => a.done - b.done);
});

function deleteItem(item) {
  itemList.value = itemList.value.filter(a => a != item)
}



function addProduct() {
  if (input_content.value === '') return;
  
  itemList.value.push({
    product: input_content.value,
    createdAt: new Date().getTime(),
    done: false
  })
  input_content.value = '';

}

watch(itemList, (newItem) => {
  localStorage.setItem('productsList', JSON.stringify(newItem));
}, {deep : true});

onMounted(()=> {
  itemList.value = JSON.parse(localStorage.getItem('productsList') || '[]') ;
})
</script>

<template>
<main class="app">
  <section class="create-product">
    <form @submit.prevent="addProduct">
      <h2 class="create-product__title">Введите продукт</h2>
      <input type="text" class="create-product__input"
      placeholder="введите название продукта"
      v-model="input_content"
       />

      <input type="submit" value="Добавить">
    </form>
  </section>
  <section class="product-list">
    <h3>Product List</h3>
    <div class="product-list__container">
      <div v-for="product in sorted_productsList" :key="product.createdAt" :class="`product-list__item ${product.done && 'done'}`">
        <label>
          <input type="checkbox" v-model="product.done">
          <span class="circle"></span>
        </label>
        <input type="text" v-model="product.product">
        <div class="delete" @click='deleteItem(product)'>&times</div>
      </div>

    </div>

  </section>
</main>

</template>


<style>
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
