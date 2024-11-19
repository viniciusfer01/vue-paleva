<script setup>
import { ref } from 'vue'
import OrderDetails from './OrderDetails.vue';

const DUMMY_ORDERS = [
  {
    id: 1,
    name: 'Zezin',
    phone: '123-456-7890',
    email: 'dummy@email.com',
    cpf: '123.456.789-00',
    status: 'pending',
    code: 'ABC123VS',
    price: 50.00,
    userId: 1,
    storeId: 1,
    createdAt: '2021-09-01T12:00:00',
    updatedAt: '2021-09-01T12:00:00',
  },
  {
    id: 2,
    name: 'Jorgin',
    phone: '123-456-7890',
    email: 'Jorgin@email.com',
    cpf: '123.456.789-00',
    status: 'pending',
    code: 'ABC456VS',
    price: 30.00,
    userId: 2,
    storeId: 1,
    createdAt: '2021-09-01T12:00:00',
    updatedAt: '2021-09-01T12:00:00',
  },
]

const props = defineProps({
  storeCode: String
})

const selectedOrder = ref(null);

const handleOrderClick = (order) => {
  selectedOrder.value = order.code;
}

const handleGoBack = () => {
  selectedOrder.value = null;
};
</script>

<template>
  <div v-if="!selectedOrder">
    <h2>Pedidos para a loja: {{ storeCode }}</h2>
    <ul>
      <li v-for="order in DUMMY_ORDERS" :key="order.id">
        <!-- <p>{{ order.name }}</p>
        <p>{{ order.phone }}</p>
        <p>{{ order.email }}</p>
        <p>{{ order.cpf }}</p> -->
        <p class="link" @click="handleOrderClick(order)" style="cursor: pointer;">{{ order.code }}</p>
        <p>{{ order.status }}</p>
        <p>{{ order.price }}</p>
        <!-- <p>{{ order.userId }}</p>
        <p>{{ order.storeId }}</p> -->
        <p>{{ order.createdAt }}</p>
        <p>{{ order.updatedAt }}</p>
      </li>
    </ul>
  </div>

  <div class="card" v-if="selectedOrder">
    <OrderDetails :orderCode="selectedOrder" :storeCode="storeCode" @goBack="handleGoBack" />
  </div>
</template>

<style scoped>
  .link {
    color: blue;
  }
  .card {
    border: 1px solid black;
    border-radius: 2rem;
    padding: 1rem;
  }
</style>
