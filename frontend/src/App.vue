<template>
  <div>
    <nav>
      <router-link to="/">Задачі</router-link>
      <span v-if="!isAuth">
        <router-link to="/login">Вхід</router-link>
        <router-link to="/register">Реєстрація</router-link>
      </span>
      <span v-else>
        <a href="#" @click.prevent="logout">Вихід</a>
      </span>
    </nav>
    <router-view></router-view>
  </div>
</template>

<script setup>
import { computed } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();
const isAuth = computed(() => !!localStorage.getItem('access_token'));

const logout = () => {
  localStorage.removeItem('access_token');
  localStorage.removeItem('refresh_token');
  window.location.reload(); 
}
</script>
