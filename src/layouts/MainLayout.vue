<template>
  <div class="app-main-layout">
    <Navbar @toggle-sidebar="isOpen = !isOpen" />

    <Sidebar :isOpen="isOpen" :key="locale" />

    <main class="app-content" :class="{full: !isOpen}">
      <Loader v-if="loading" />
      <div class="app-page" v-else>
        <router-view />
      </div>
    </main>

    <div class="fixed-action-btn">
      <router-link
        class="btn-floating btn-large blue"
        to="/record"
        v-tooltip="'MainLayout_CreateNewRecord'"
      >
        <i class="large material-icons">add</i>
      </router-link>
    </div>
  </div>
</template>

<script>
import Navbar from '@/components/app/Navbar'
import Sidebar from '@/components/app/Sidebar'
import messages from '@/common/messages'

export default {
  name: 'main-layout',

  components: {
    Navbar,
    Sidebar
  },

  data: () => ({
    isOpen: true,
    loading: true
  }),

  computed: {
    error() {
      return this.$store.getters.error
    },
    locale() {
      return this.$store.getters.info.locale
    }
  },

  watch: {
    error(firebaseError) {
      // $error - custom property from @/common/message.plugin.js
      this.$error(messages[firebaseError.code] || 'Что-то пошло не так :(')
    }
  },

  async mounted() {
    if (!Object.keys(this.$store.getters.info).length) {
      await this.$store.dispatch('fetchInfo')
    }

    this.loading = false
  }
}
</script>
