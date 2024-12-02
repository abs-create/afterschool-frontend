<template>
  <div class="app">
    <h1>Afterschool Activities</h1>

    <!-- Sort By Dropdown -->
    <div>
      <label>Sort by:</label>
      <select v-model="sortBy">
        <option value="topic">Subject</option>
        <option value="location">Location</option>
        <option value="price">Price</option>
        <option value="space">Spaces</option>
      </select>
      <button @click="sortLessons">Sort</button>
    </div>

    <!-- Lesson List -->
    <div v-if="lessons.length > 0">
      <div v-for="lesson in lessons" :key="lesson._id" class="lesson">
        <h3>{{ lesson.topic }}</h3>
        <p>Location: {{ lesson.location }}</p>
        <p>Price: £{{ lesson.price }}</p>
        <p>Spaces: {{ lesson.space }}</p>
        <button
          v-if="lesson.space > 0"
          @click="addToCart(lesson)"
          :disabled="lesson.space === 0"
        >
          Add to Cart
        </button>
      </div>
    </div>
    <p v-else>No lessons available.</p>

    <!-- Cart -->
    <div v-if="showCart && cart.length > 0">
      <h2>Your Cart</h2>
      <div v-for="(item, index) in cart" :key="index" class="cart-item">
        <p>{{ item.topic }} - £{{ item.price }}</p>
        <button @click="removeFromCart(item)">Remove</button>
      </div>
      <button @click="checkout">Checkout</button>
    </div>

    <!-- Show Cart Button -->
    <button v-if="cart.length > 0" @click="toggleCart">Go to Cart</button>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      lessons: [], // Fetch lessons dynamically
      cart: [], // Shopping cart
      sortBy: "topic", // Default sort option
      showCart: false, // Cart visibility toggle
    };
  },
  created() {
    this.fetchLessons();
  },
  methods: {
    // Fetch lessons from backend
    async fetchLessons() {
      try {
        const response = await fetch("http://localhost:3000/activities");
        if (!response.ok) throw new Error("Failed to fetch lessons");
        const data = await response.json();
        console.log("Lessons fetched:", data);
        this.lessons = data;
      } catch (error) {
        console.error("Error fetching lessons:", error);
      }
    },

    // Sorting lessons
    sortLessons() {
      this.lessons.sort((a, b) => {
        if (this.sortBy === "price" || this.sortBy === "space") {
          return a[this.sortBy] - b[this.sortBy];
        }
        return a[this.sortBy].localeCompare(b[this.sortBy]);
      });
    },

    // adding a lesson to cart
    addToCart(lesson) {
      this.cart.push(lesson);
      lesson.space--; // Decrease space count
    },

    // Toggle cart 
    toggleCart() {
      this.showCart = !this.showCart;
    },

    // removing a lesson from a cart
    removeFromCart(item) {
      this.cart = this.cart.filter((i) => i !== item);
      item.space++; // Increase space count
    },

    // once checkout it will pop up the message that its checked out
    checkout() {
      alert("Your order has been placed!");
      this.cart = []; // Clear cart
      this.showCart = false; // Close cart
    },
  },
};
</script>

<style>
.app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.lesson {
  margin: 10px 0;
}

.cart-item {
  margin: 10px 0;
}

button {
  margin-top: 10px;
}
</style>
