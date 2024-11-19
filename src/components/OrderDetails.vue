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
  }
};

onMounted(() => {
  fetchOrderDetails();
});
</script>

<template>
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
    <button @click="handleGoBack">Back to Orders</button>
  </div>
</template>