<template>

  <div>
    <h2>Store Reviews</h2>

    <p v-if="reviews.length === 0">There are no reviews yet.</p>

    <ul class="review-list">
      <li v-for="review in reviews" :key="review.name">
        <p>Name: {{ review.name }}</p>
        <p>Review: {{ review.review }}</p>
        <p>Rating: {{ review.rating }}</p>
      </li>
    </ul>

    <div v-if="errors.length !== 0">
      <h4 class="error-title">Please correct the following errors:</h4>
      <ul class="error-list">
        <li v-for="error in errors" :key="error.name">{{ error.name + ' ' + error.msg }}</li>
      </ul>
    </div>

    <form @submit.prevent="onSubmit">
      <p>
        <label>Name:</label>
        <input class="input" placeholder="Enter your name" v-model="name">
      </p>
      <p>
        <label>Review:</label>
        <textarea class="input" placeholder="Enter your review" v-model="review"></textarea>
      </p>
      <p>
        <label>Rating:</label>
        <select v-model.number="rating">
          <option>5</option>
          <option>4</option>
          <option>3</option>
          <option>2</option>
          <option>1</option>
        </select>
      </p>
      <p> <input type="submit"></p>
    </form>
  </div>

</template>

<script>
export default {
  name: 'StoreReviews',
  props: ['reviews'],
  data() {
    return {
      name: '',
      review: '',
      rating: '',
      errors: [],
    };
  },
  methods: {
    onSubmit() {
      // model keys defs
      const keys = [
        'name',
        'review',
        'rating',
      ];

      // init emit object
      const productReview = {};

      // input sanitization
      this.errors = [];
      keys.forEach((e) => {
        if (this[e] === '') this.errors.push({ name: e, msg: 'required' });
      });

      if (this.errors.length !== 0) {
        return;
      }

      // populate emit object with model vals
      keys.forEach((e) => {
        productReview[e] = this[e];
      });
      this.$emit('reviewSubmitted', productReview);

      // wipe model vals
      keys.forEach((e) => { this[e] = null; });
    },
  },
  computed: {
    // ...
  },
};
</script>

<style scoped>
textarea {
  width: 80%;
  height: 6em;
}
.input {
  font-size: 1.25em;
  border-radius: 0.25em;
  border: 1px solid grey;
  padding: 0.5em;
}

.review-list,
.error-list {
  list-style: none;
  padding-left: 0;
}
.error-list {
  margin-top: 0.5em;
}
.error-title {
  color: red;
  margin-bottom: 0;
}
form label {
  display: block;
}
</style>
