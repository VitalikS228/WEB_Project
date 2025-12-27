<template>
  <div class="card">
    <h2>Вхід</h2>
    <form @submit.prevent="handleLogin">
      <input v-model="username" placeholder="Username" required /><br>
      <input type="password" v-model="password" placeholder="Password" required /><br>
      <button>Увійти</button>
      <p v-if="error" class="error">{{ error }}</p>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import axios from 'axios';

const username = ref('');
const password = ref('');
const error = ref('');
const router = useRouter();

const handleLogin = async () => {
  try {
    const res = await axios.post('http://localhost:8000/api/auth/login/', {
      username: username.value,
      password: password.value
    });
    localStorage.setItem('access_token', res.data.access);
    localStorage.setItem('refresh_token', res.data.refresh);
    window.location.href = '/'; 
  } catch (e) {
    error.value = 'Помилка входу. Перевірте дані.';
  }
}
</script>
