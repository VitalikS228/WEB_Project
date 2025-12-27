<template>
  <div>
    <div class="card">
      <h3>Нова задача</h3>
      <form @submit.prevent="add">
        <input v-model="newTask.title" placeholder="Назва задачі" required style="width: 100%" />
        <textarea v-model="newTask.description" placeholder="Текст задачі" style="width: 100%; margin-top: 5px;"></textarea>
        
        <div style="display: flex; gap: 10px; margin-top: 5px;">
          <label>Початок: <input type="date" v-model="newTask.start_date" /></label>
          <label>Кінець: <input type="date" v-model="newTask.end_date" /></label>
        </div>
        
        <button type="submit" style="margin-top: 10px;">Додати задачу</button>
      </form>
    </div>

    <div v-for="t in tasks" :key="t.id" class="card">
      <div style="display: flex; justify-content: space-between;">
        <h4>{{ t.title }}</h4>
        <button @click="deleteTask(t.id)" style="background: red; padding: 5px;">X</button>
      </div>
      <p>{{ t.description }}</p>
      <small>Створено: {{ new Date(t.created_at).toLocaleDateString() }}</small>
      <div v-if="t.start_date || t.end_date">
        <small>Термін: {{ t.start_date }} - {{ t.end_date }}</small>
      </div>
      
      <div style="margin-top: 10px;">
        <select :value="t.status" @change="updateStatus(t, $event.target.value)">
          <option value="todo">Зробити</option>
          <option value="in_progress">В роботі</option>
          <option value="done">Виконана</option>
          <option value="archived">Архівована</option>
        </select>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const tasks = ref([]);
const newTask = ref({ title: '', description: '', start_date: '', end_date: '' });

const api = axios.create({ 
  baseURL: 'http://localhost:8000/api/', 
  headers: { Authorization: `Bearer ${localStorage.getItem('access_token')}` } 
});

const load = async () => { 
  try { const r = await api.get('tasks/'); tasks.value = r.data; } catch(e){} 
};

const add = async () => { 
  if(!newTask.value.title) return; 
  await api.post('tasks/', newTask.value); 
  newTask.value = { title: '', description: '', start_date: '', end_date: '' }; 
  load(); 
};

const updateStatus = async (task, newStatus) => {
  await api.patch(`tasks/${task.id}/`, { status: newStatus });
  load();
};

const deleteTask = async (id) => {
  if(confirm('Видалити?')) {
    await api.delete(`tasks/${id}/`);
    load();
  }
};

onMounted(load);
</script>
