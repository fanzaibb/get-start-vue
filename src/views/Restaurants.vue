<template>
  <div class="container py-5">
    <NavTabs />
    <!-- 餐廳類別標籤 RestaurantsNavPills -->
    <RestaurantNavPills 
      :categories='categories'/>
    <div class="row">
      <!-- 餐廳卡片 RestaurantCard-->
      <RestaurantCard 
        v-for="restaurant in restaurants"
        :key="restaurant.id"
        :initial-restaurant="restaurant"
        />
    </div>

    <!-- 分頁標籤 RestaurantPagination -->
    <RestaurantPagination 
      v-if="totalPage > 1"
      :category-id="categoryId"
      :current-page="currentPage"
      :total-page="totalPage"
      />
  </div>
</template>

<script>
import NavTabs from '../components/NavTabs'
import RestaurantCard from '../components/ResraurantCard'
import RestaurantNavPills from '../components/RestaurantNavPills'
import RestaurantPagination from '../components/RestaurantPagination'
import restaurantsAPI from './../apis/restaurants'
import { Toast } from './../utils/helpers'


export default {
  name: 'Restaurants',
  components: {
    NavTabs,
    RestaurantCard,
    RestaurantNavPills,
    RestaurantPagination

  },
  data () {
    return {
      categories: [],
      categoryId: -1,
      currentPage: 1,
      restaurants: [],
      totalPage: -1
    }
  },
  created () {
    const { page, categoryId } = this.$route.query
    this.fetchRestaurants({ page, categoryId })
  },
  beforeRouterUpdate (to, from, next){
    console.log('to', to)
    console.log('from', from)
    const { page, categoryId } = to.query
    this.fetchRestaurants({ page, categoryId })
    next()
  },
  methods: {
    async fetchRestaurants ({ page = 1 , categoryId = '' }) {
      try {
        const { data, statusText } = await restaurantsAPI.getRestaurants({
          page, categoryId
        })
        // console.log('response', response)

        if (statusText !== 'OK') {
          throw new Error(statusText)
        }
        this.categories = data.categories
        this.categoryId = data.categoryId
        this.currentPage = data.page
        this.restaurants = data.restaurants
        this.totalPage = data.totalPage.length
      
      } catch (error) {
        Toast.fire({
          type: 'error',
          title: '無法取得餐廳資料，請稍後再試'
        })
        console.log('error', error)
      }
    }
  }
}


</script>