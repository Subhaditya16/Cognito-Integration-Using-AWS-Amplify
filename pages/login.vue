<template>
  <div class="min-h-full flex items-center justify-center py-12 px-4 sm:px-6 lg:px-8">
  <div class="max-w-md w-full space-y-8">
    <logo />
    <div>
      <h2 class="mt-6 text-center text-3xl font-extrabold text-gray-900">Log in to your account</h2>
      <p class="mt-2 text-center text-sm text-gray-600"></p>
    </div>
    <div v-if="!$auth.isAuthenticated">
      <form class="mt-8 space-y-6" @submit.prevent="login">
      <div class="rounded-md shadow-sm -space-y-px">
        <div>
          <label for="email-address" class="sr-only">Email address</label>
          <input v-model="form.email" type="email" required class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-t-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm" placeholder="Email">
        </div>
        <div>
          <label for="password" class="sr-only">Password</label>
          <input v-model="form.password" type="password" required class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-b-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm" placeholder="Password">
        </div>
      </div>
      <div>
        <button type="submit" class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
          <span class="absolute left-0 inset-y-0 flex items-center pl-3"></span>
          Log in
        </button>
      </div>
    </form>
    <div>
      <button type="submit" class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
        <nuxt-link to="/register">
          <span class="absolute left-0 inset-y-0 flex items-center pl-3"></span>
          Register
        </nuxt-link>  
      </button>
    </div>
    <div>
        <button v-on:click ="facebook" class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
          <span class="absolute left-0 inset-y-0 flex items-center pl-3"></span>
          Sign Up with Facebook
        </button>
    </div>
    <div>
        <button v-on:click ="google" class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-red-600 hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
          <span class="absolute left-0 inset-y-0 flex items-center pl-3"></span>
          Sign Up with Google
        </button>
    </div>
    </div>
    
    <div v-else>
      You're logged in as {{ $auth.email }}.
        <button @click="$store.dispatch('auth/logout')" class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-red-600 hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
          <span class="absolute left-0 inset-y-0 flex items-center pl-3"></span>
          Log Out
        </button>
    </div>
  </div>
</div>
</template>

<script>
import Logo from '~/components/Logo.vue'
import { Auth } from 'aws-amplify'

export default {
  components: {
    Logo
  },
  data: () => ({
    form: {
      email: '',
      password: ''
    }
  }),
  methods: {
    async login() {
      try {
        await this.$store.dispatch('auth/login', this.form)
        this.$router.push('/')
      } catch (error) {
        console.log({ error })
      }
    },
    facebook() {
      try {
        Auth.federatedSignIn({ provider: 'Facebook' })
      } catch (error) {
        console.log('error:', error);
      }
    },
    google() {
      try {
        Auth.federatedSignIn({ provider: 'Google' })
      } catch (error) {
        console.log('error:', error);
      }
    }
  }
}
</script>

