<script setup>
import { ref, onMounted } from 'vue'
import OrderDetails from './OrderDetails.vue';

// const DUMMY_ORDERS = [
//   {
//     id: 1,
//     name: 'Zezin',
//     phone: '123-456-7890',
//     email: 'dummy@email.com',
//     cpf: '123.456.789-00',
//     status: 'pending',
//     code: 'ABC123VS',
//     price: 50.00,
//     userId: 1,
//     storeId: 1,
//     createdAt: '2021-09-01T12:00:00',
//     updatedAt: '2021-09-01T12:00:00',
//   },
//   {
//     id: 2,
//     name: 'Jorgin',
//     phone: '123-456-7890',
//     email: 'Jorgin@email.com',
//     cpf: '123.456.789-00',
//     status: 'pending',
//     code: 'ABC456VS',
//     price: 30.00,
//     userId: 2,
//     storeId: 1,
//     createdAt: '2021-09-01T12:00:00',
//     updatedAt: '2021-09-01T12:00:00',
//   },
// ]

const orders = ref([]);
const selectedOrder = ref(null);

const props = defineProps({
  storeCode: String
})
const handleOrderClick = (order) => {
  selectedOrder.value = order.code;
}

const handleGoBack = () => {
  selectedOrder.value = null;
};

const fetchOrders = async () => {
  try {
    const response = await fetch(`http://localhost:3000/api/orders?code=${props.storeCode}`);
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    const data = await response.json();
    orders.value = data;
    console.log('orders:', orders.value);
  } catch (error) {
    console.error('There was a problem with the fetch operation:', error);
  }
};

onMounted(() => {
  fetchOrders();
});
</script>

<template>
  <div v-if="!selectedOrder">
    <h2>Pedidos para a loja: {{ storeCode }}</h2>
    <ul>
      <li v-for="order in orders" :key="order.id">
        <p class="link" @click="handleOrderClick(order)" style="cursor: pointer;">{{ order.code }}</p>
        <p>{{ order.status }}</p>
        <p>{{ order.price }}</p>
        <p>{{ order.created_at }}</p>
        <p>{{ order.updated_at }}</p>
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
