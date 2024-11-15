<script setup>
import { defineProps } from 'vue'

const props = defineProps({
  orderCode: String,
  storeCode: String
})

const emit = defineEmits(['goBack']);

const handleGoBack = () => {
  emit('goBack');
};

const orderDetails = {
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
}
</script>

<template>
  <div>
    <h2>Order Details for: {{ orderCode }}</h2>
    <p>Store: {{ storeCode }}</p>
    <p>Name: {{ orderDetails.name }}</p>
    <p>Phone: {{ orderDetails.phone }}</p>
    <p>Email: {{ orderDetails.email }}</p>
    <p>CPF: {{ orderDetails.cpf }}</p>
    <p>Status: {{ orderDetails.status }}</p>
    <p>Price: {{ orderDetails.price }}</p>
    <p>Created At: {{ orderDetails.createdAt }}</p>
    <p>Updated At: {{ orderDetails.updatedAt }}</p>
    <button @click="handleGoBack">Back to Orders</button>
  </div>
</template>