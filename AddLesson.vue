<template>
  <div>
    <h2>Add Lesson</h2>
    <form @submit.prevent="addLesson">
      <input v-model="newLesson.topic" placeholder="Topic" required />
      <input v-model="newLesson.price" placeholder="Price" type="number" required />
      <input v-model="newLesson.location" placeholder="Location" required />
      <input v-model="newLesson.space" placeholder="Space" type="number" required />
      <button type="submit">Add Lesson</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      newLesson: {
        topic: "",
        price: 0,
        location: "",
        space: 0,
      },
    };
  },
  methods: {
    async addLesson() {
      try {
        await axios.post("http://localhost:3000/activities", this.newLesson);
        alert("Lesson added successfully!");
        this.$emit("lesson-added"); // Notify parent to refresh lessons
        this.newLesson = { topic: "", price: 0, location: "", space: 0 }; // Reset form
      } catch (error) {
        console.error("Error adding lesson:", error);
      }
    },
  },
};
</script>

<style scoped>
h2 {
  text-align: center;
  margin-bottom: 10px;
}
form {
  display: flex;
  flex-direction: column;
  align-items: center;
}
input {
  margin: 5px 0;
  padding: 5px;
  width: 200px;
}
button {
  margin-top: 10px;
  padding: 5px 15px;
}
</style>
