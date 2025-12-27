<template>
  <div class="card">
    <h2>Реєстрація</h2>
    <form @submit.prevent="handleRegister">
      <input v-model="username" placeholder="Username" required /><br>
      <input v-model="email" placeholder="Email" type="email" /><br>
      <input type="password" v-model="password" placeholder="Password" required /><br>
      <button>Зареєструватися</button>
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
const email = ref('');
const error = ref('');
const router = useRouter();

const handleRegister = async () => {
  try {
    await axios.post('http://localhost:8000/api/auth/register/', {
      username: username.value,
      password: password.value,
      email: email.value
    });
    router.push('/login');
  } catch (e) {
    error.value = 'Помилка реєстрації';
  }
}
</script>
