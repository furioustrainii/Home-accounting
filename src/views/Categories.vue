<template>
  <div>
    <div class="page-title">
      <h3>{{'Categories_Categories' | localize}}</h3>
    </div>
    <section>
      <Loader v-if="loading" />

      <div class="row" v-else>
        <CategoryCreate @category-created="addNewCategory" />

        <CategoryEdit
          v-if="categories.length"
          :categories="categories"
          :key="categories.length + updateCount"
          @category-updated="updateCategory"
        />

        <p class="center" v-else>{{ 'Categories_NoCategoriesYet' | localize }}</p>
      </div>
    </section>
  </div>
</template>


<script>
import CategoryCreate from '@/components/CategoryCreate'
import CategoryEdit from '@/components/CategoryEdit'

export default {
  name: 'categories',
  metaInfo() {
    return {
      title: this.$title('Title_Categories')
    }
  },

  components: {
    CategoryCreate,
    CategoryEdit
  },

  data: () => ({
    loading: true,
    updateCount: 0,
    categories: []
  }),

  methods: {
    addNewCategory(category) {
      this.categories.push(category)
    },

    updateCategory(updatedCategory) {
      const updatedCategoryID = this.categories.findIndex(
        category => category.id === updatedCategory.id
      )
      this.categories[updatedCategoryID].title = updatedCategory.title
      this.categories[updatedCategoryID].limit = updatedCategory.limit

      this.updateCount++
    }
  },

  async mounted() {
    this.categories = await this.$store.dispatch('fetchCategories')
    this.loading = false
  }
}
</script>
