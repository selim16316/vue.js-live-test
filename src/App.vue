<script>
import { ref } from 'vue';

export default {
  name: 'TaskList',
  setup() {
    const tasks = ref([
      { name: 'Task 1', time: 30 },
      { name: 'Task 2', time: 40 },
      { name: 'Task 3', time: 60 },
      { name: 'Task 4', time: 45 },
      { name: 'Task 5', time: 50 },
    ]);

    const isEditFormVisible = ref(false);
    const editedTask = ref({ name: '', time: 0 });
    const editedTaskIndex = ref(null);

    const openEditForm = (index) => {
      editedTask.value = { ...tasks.value[index] };
      editedTaskIndex.value = index;
      isEditFormVisible.value = true;
    };

    const updateTask = () => {
      if (editedTaskIndex.value !== null) {
        tasks.value[editedTaskIndex.value] = { ...editedTask.value };
        closeEditForm();
      }
    };

    const closeEditForm = () => {
      isEditFormVisible.value = false;
      editedTaskIndex.value = null;
    };

    return {
      tasks,
      isEditFormVisible,
      editedTask,
      openEditForm,
      updateTask,
      closeEditForm,
    };
  },
};
</script>



<template>
  <div>
    <h1> Live Test</h1>
    <ul>
      <li v-for="(task, index) in tasks" :key="index" class="task-item">
        {{ task.name }} - {{ task.time }}
        <button @click="openEditForm(index)" class="edit-button">Edit</button>
      </li>
    </ul>

    <div v-if="isEditFormVisible" class="popup">
      <form @submit.prevent="updateTask">
        <label>
          Name:
          <input v-model="editedTask.name" type="text" required />
        </label>
        <label>
          Time:
          <input v-model.number="editedTask.time" type="number" required />
        </label>
        <button type="submit">Submit</button>
      </form>
      <button @click="closeEditForm">Close</button>
    </div>
  </div>
</template>



<style scoped>

.task-item {
  width: 300px;
  background-color: #9CD;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border: 1px solid #ccc;
  padding: 10px;
  margin-bottom: 8px;

}

.edit-button {
  padding: 5px 10px;
  border: 1px solid #ccc;
  background-color: #fff;
  cursor: pointer;
}

.popup {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 20px;
  background-color: white;
  border: 1px solid #ccc;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  z-index: 1000;
}
</style>
