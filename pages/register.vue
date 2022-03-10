<template>
  <div class="min-h-full flex items-center justify-center py-12 px-4 sm:px-6 lg:px-8">
  <div class="max-w-md w-full space-y-8">
    <logo />
    <div>
      <h2 class="mt-6 text-center text-3xl font-extrabold text-gray-900">Sign up to your account</h2>
      <p class="mt-2 text-center text-sm text-gray-600"></p>
    </div>
    <div v-if="!$auth.isAuthenticated">
      <form class="mt-8 space-y-6"  v-if="step === steps.register" @submit.prevent="register">
      <div class="rounded-md shadow-sm -space-y-px">
        <div>
          <label for="email-address" class="sr-only">Email address</label>
          <input v-model="registerForm.email" type="email" required class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-t-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm" placeholder="Email">
        </div>
        <div>
          <label for="password" class="sr-only">Password</label>
          <input v-model="registerForm.password" type="password" required class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-b-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm" placeholder="Password">
        </div>
      </div>

      <div>
        <button type="submit" class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
          <span class="absolute left-0 inset-y-0 flex items-center pl-3"></span>
          Register
        </button>
      </div>
      </form>
      <form class="mt-8 space-y-6" v-else @submit.prevent="confirm">
      <div class="rounded-md shadow-sm -space-y-px">
        <div>
          <label for="email-address" class="sr-only">Email address</label>
          <input v-model="confirmForm.email" type="email" required class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-t-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm" placeholder="Email">
        </div>
        <div>
          <label for="password" class="sr-only">Code</label>
          <input v-model="confirmForm.code" required class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-b-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm" placeholder="Code">
        </div>
      </div>

      <div>
        <button type="submit" class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
          <span class="absolute left-0 inset-y-0 flex items-center pl-3"></span>
          Register
        </button>
    </div>
    </form>
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
import Logo from '~/components/Logo.vue';
// import { Auth } from 'aws-amplify';

const steps = {
  register: 'REGISTER',
  confirm: 'CONFIRM'
}

export default {
  data: () => ({
    steps: { ...steps },
    step: steps.register,
    registerForm: {
      email: '',
      password: ''
    },
    confirmForm: {
      email: '',
      code: ''
    }
  }),
  components: {
    Logo
  },
  methods: {
    async register() {
      try {
        await this.$store.dispatch('auth/register', this.registerForm)
        this.confirmForm.email = this.registerForm.email
        this.step = this.steps.confirm
      } catch (error) {
        console.log({ error })
      }
    },
    async confirm() {
      try {
        await this.$store.dispatch('auth/confirmRegistration', this.confirmForm)
        await this.$store.dispatch('auth/login', this.registerForm)
        this.$router.push('/')
      } catch (error) {
        console.log({ error })
      }
    },
  }
}
</script>


