<template>
  <div class="min-h-full flex items-center justify-center py-12 px-4 sm:px-6 lg:px-8">
  <div class="max-w-md w-full space-y-8">
    <logo />
      <div>
        <button type="submit" class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
          <span class="absolute left-0 inset-y-0 flex items-center pl-3"></span>
          <nuxt-link to="/login">
          Log in
          </nuxt-link>
        </button>
      </div>
    <div>
      <button type="submit" class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
        <nuxt-link to="/register">
          <span class="absolute left-0 inset-y-0 flex items-center pl-3"></span>
          Register
        </nuxt-link>  
      </button>
    </div>
  </div>
</div>
</template>

<script>
import Logo from '~/components/Logo.vue'

export default {
  layout: 'default',
  components: {
    Logo
  },
}
</script>

