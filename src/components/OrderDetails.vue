<script setup>
import { ref, onMounted } from 'vue';
import { defineProps } from 'vue'

const props = defineProps({
  orderCode: String,
  storeCode: String
})

const emit = defineEmits(['goBack']);

const handleGoBack = () => {
  emit('goBack');
};

const orderDetails = ref(null);
const errorMessage = ref('');
const successMessage = ref('');

const fetchOrderDetails = async () => {
  try {
    const response = await fetch(`http://localhost:3000/api/orders/${props.orderCode}?code=${props.storeCode}`);
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    const data = await response.json();
    orderDetails.value = data;
  } catch (error) {
    console.error('There was a problem with the fetch operation:', error);
    errorMessage.value = 'Houve um problema ao obter os detalhes do pedido. Tente novemente.';
  }
};

const startPrepping = async () => {
  try {
    const response = await fetch(`http://localhost:3000/api/orders/${props.orderCode}/prepping?code=${props.storeCode}`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      }
    });
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    const data = await response.json();
    orderDetails.value = data; // Update order details with the response data
    successMessage.value = 'Status do pedido alterado com sucesso!';
  } catch (error) {
    console.error('There was a problem with the fetch operation:', error);
    errorMessage.value = 'Houve um problema ao alterar o status do pedido. Tente novemente.';
  }
};

const readyOrder = async () => {
  try {
    const response = await fetch(`http://localhost:3000/api/orders/${props.orderCode}/ready?code=${props.storeCode}`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      }
    });
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    const data = await response.json();
    orderDetails.value = data; // Update order details with the response data
    successMessage.value = 'Status do pedido alterado com sucesso!';
  } catch (error) {
    console.error('There was a problem with the fetch operation:', error);
    errorMessage.value = 'Houve um problema ao alterar o status do pedido. Tente novemente.';
  }
};

onMounted(() => {
  fetchOrderDetails();
});
</script>

<template>
  <div v-if="errorMessage">
    <p>{{ errorMessage }}</p>
  </div>
  <div v-if="successMessage">
    <p>{{ successMessage }}</p>
  </div>
  <div v-if="orderDetails">
    <h2>Order Details for: {{ orderCode }}</h2>
    <p>Store: {{ storeCode }}</p>
    <p>Name: {{ orderDetails.order.name }}</p>
    <p>Phone: {{ orderDetails.order.phone }}</p>
    <p>Email: {{ orderDetails.order.email }}</p>
    <p>CPF: {{ orderDetails.order.cpf }}</p>
    <p>Status: {{ orderDetails.order.status }}</p>
    <p>Price: {{ orderDetails.order.price }}</p>
    <p>Created At: {{ orderDetails.order.created_at }}</p>
    <p>Updated At: {{ orderDetails.order.updated_at }}</p>
    <button class="card" @click="handleGoBack">Back to Orders</button>
    <div class="card" v-if="orderDetails.order.status === 'pending'">
      <button @click="startPrepping">Iniciar Preparo</button>
    </div>
    <div class="card" v-if="orderDetails.order.status === 'prepping'">
      <button @click="readyOrder">Pedido Pronto</button>
    </div>
  </div>

</template>