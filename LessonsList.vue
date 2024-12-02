<template>
  <div>
    <h2>Lessons List</h2>
    <ul>
      <li v-for="lesson in lessons" :key="lesson._id">
        {{ lesson.topic }} - {{ lesson.location }} - £{{ lesson.price }} - Space: {{ lesson.space }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    lessons: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style scoped>
h2 {
  text-align: center;
  margin-bottom: 10px;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin: 5px 0;
}
</style>
