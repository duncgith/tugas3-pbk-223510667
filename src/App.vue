<template>
  <div class="task-app">
    <h1>Sith</h1>
    <h2>the force</h2>
    <ul>
      <li v-for="(activity, index) in filteredActivities" :key="index" class="task-item">
        <div class="checkbox-left">
          <input type="checkbox" v-model="activity.completed">
          <span :class="{ 'completed': activity.completed }">{{ activity.name }}</span>
        </div>
        <button @click="editActivity(index)" class="edit-button">Edit</button>
        <button @click="cancelActivity(index)" class="delete-button">Batalkan</button>
      </li>
    </ul>
    <form @submit.prevent="addOrUpdateActivity" class="add-task-form">
      <input type="text" v-model="newActivity" placeholder="Tambah kegiatan baru atau edit">
      <button type="submit">{{ isEditing ? 'Update' : 'Tambah' }}</button>
      <button v-if="isEditing" @click="cancelEdit" type="button">Batalkan Edit</button>
    </form>
    <button @click="filterCompleted" class="filter-button">{{ showCompleted ? 'Tampilkan semua' : 'Tampilkan Belum Selesai' }}</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      activities: [
        { name: 'darth maul', completed: false },
        { name: 'anakin skywalker', completed: true },
      ],
      newActivity: '',
      showCompleted: false,
      isEditing: false,
      currentIndex: null,
    };
  },
  methods: {
    addOrUpdateActivity() {
      if (this.newActivity.trim()) {
        if (this.isEditing) {
          this.activities[this.currentIndex].name = this.newActivity.trim();
          this.isEditing = false;
          this.currentIndex = null;
        } else {
          this.activities.push({ name: this.newActivity.trim(), completed: false });
        }
        this.newActivity = '';
      }
    },
    cancelActivity(index) {
      this.activities.splice(index, 1);
    },
    editActivity(index) {
      this.newActivity = this.activities[index].name;
      this.isEditing = true;
      this.currentIndex = index;
    },
    cancelEdit() {
      this.isEditing = false;
      this.newActivity = '';
      this.currentIndex = null;
    },
    filterCompleted() {
      this.showCompleted = !this.showCompleted;
    },
  },
  computed: {
    filteredActivities() {
      if (this.showCompleted) {
        return this.activities.filter(activity => !activity.completed);
      } else {
        return this.activities;
      }
    },
  },
};
</script>

<style>
body {
  background-image: url('./assets/e89d80817498f9e33040e9c2b61ecc06.jpeg');
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
}

.task-app {
  font-family: 'stencil', cursive;
  margin: 200px;
  border: 10px solid black;
}

h1, h2 {
  color: rgb(255, 254, 254);
}

ul {
  list-style-type: none;
  padding: 0;
}

.completed {
  text-decoration: line-through;
}

.delete-button, .edit-button {
  background-color: red;
  color: white;
  border: none;
  padding: 10px;
  cursor: pointer;
  border-radius: 4px;
  margin-left: 5px;
}

.edit-button {
  background-color: blue;
}

.task-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 10px;
}

.checkbox-left {
  margin-right: 10px;
}

.add-task-form {
  margin-top: 20px;
}

.filter-button {
  margin-top: 10px;
}
</style>
