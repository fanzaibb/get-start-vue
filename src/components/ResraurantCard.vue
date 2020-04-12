<template>
  <div class="col-md-6 col-lg-4">
    <div class="card mb-4">
      <img
        class="card-img-top"
        src="https://via.placeholder.com/300"
        alt="Card image cap"
        width="286px"
        height="180px"
      >
      <div class="card-body">
        <p class="card-text title-wrap">
          <router-link to="{ name: 'restaurant', params: { id: restaurant.id }}">
            {{ restaurant.name }}
          </router-link>
        </p>
        <span class="badge badge-secondary">{{ restaurant.Category.name }}</span>
        <p class="card-text text-truncate">
          {{ restaurant.description }}
        </p>
      </div>
      <div class="card-footer">
        <button
          v-if="restaurant.isFavorited"
          type="button"
          class="btn btn-danger btn-border favorite mr-2"
          @click.stop.prevent="deleteFavorite(restaurant.id)"
        >
          移除最愛
        </button>
        <button
          v-else
          type="button"
          class="btn btn-primary btn-border favorite mr-2"
          @click.stop.prevent="addFavorite(restaurant.id)"
        >
          加到最愛
        </button>
        <button
          v-if="restaurant.isLiked"
          type="button"
          class="btn btn-danger like mr-2"
          @click.stop.prevent="deleteLike"
        >
          Unlike
        </button>
        <button
          v-else
          type="button"
          class="btn btn-primary like mr-2"
          @click.stop.prevent="addLike"
        >
          Like
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import usersAPI from './../apis/user'
import { Toast } from './../utils/helpers'
export default {
  name: "RestaurantCard",
  props: {
    initialRestaurant: {
      type: Object,
      require: true
    }
  },
  data () {
    return {
      restaurant: this.initialRestaurant
    }
  },
  methods: {
    async addFavorite (restaurantId) {
      try {
        const {data, statusText } = await usersAPI.addFavorite({ restaurantId })
        if (statusText !== 'OK' || data.status !== 'success') {
          throw new Error(statusText)
        }
        this.restaurant = {
          ...this.restaurant,
          isFavorited: true
        }
      } catch (error) {
        Toast.fire({
          type: 'error',
          title: '無法加入，請稍候重試'
        })
      }
    },
    async deleteFavorite (restaurantId) {
      try {
        const { data, statusText } = await usersAPI.deleteFavorite({ restaurantId })
        if (statusText !== 'OK' || data.status !== 'success') {
          throw new Error(statusText)
        }
        this.restaurant = {
          ...this.restaurant,
          isFavorited: false
        }
      } catch (error) {
        Toast.fire({
          type: 'error',
          title: '移除失敗，請稍後重試'
        })
      }
    },
    addLike () {
      this.restaurant = {
        ...this.restaurant,
        isLiked: true
      }
    },
    deleteLike () {
      this.restaurant = {
        ...this.restaurant,
        isLiked: false
      }
    }
  }
}
</script>