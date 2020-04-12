<template>
  <form @submit.stop.prevent="handleSubmit">
    <div class="form-group">
      <label for="text">留下評論：</label>
      <textarea
        v-model="text"
        class="form-control"
        rows="3"
        name="text"
      />
    </div>
    <div class="text-right">
      <button
        type="submit"
        class="btn btn-primary mr-0"
      >
        Submit
      </button>
    </div>
  </form>
</template>

<script>
import uuid from 'uuid/v4'

export default {
  name: 'CreateComment',
  data () {
    return {
      text: ''
    }
  },
  props: {
    restaurantId: {
      type: Number,
      required: true
    }
  },
  methods: {
    handleSubmit () {
      console.log("submit")
      this.$emit('after-create-comment', {
        commentId: uuid(),
        restaurantId: this.restaurantId,
        text: this.text
      })
      this.text = ''
    }
  }
}
</script>