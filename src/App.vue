<script setup>
import ListOrders from './components/ListOrders.vue'
import { ref } from 'vue'

const storeCode = ref('')
const showOrdersComponent = ref(false)

const showOrders = () => {
  if (storeCode.value) {
    showOrdersComponent.value = true
  } else {
    showOrdersComponent.value = false
  }
}
</script>

<template>
  <div>
    <a href="/">
      <h1>PaLevá - Kitchen Edition</h1>
    </a>
    <div v-if="!showOrdersComponent">
      <input type="text" placeholder="Procurar por pedidos da loja..." v-model="storeCode"/>
      <button @click="showOrders">Mostrar pedidos</button>
    </div>
    <ListOrders v-if="showOrdersComponent" :storeCode="storeCode"/>
  </div>
</template>

