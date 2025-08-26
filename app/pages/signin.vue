<script setup lang="ts">
import { authClient } from '@/utils/auth-client'
// Router for redirect
const router = useRouter()
const { data: session } = await authClient.useSession(useFetch);

if (session.value?.session.userId) {
  router.push('/')
}

// Form state
const email = ref('')
const password = ref('')



const handleSubmit = async () => {
  const result = await authClient.signIn.email({
    email: email.value,
    password: password.value,
  })

  if (!result.error) {
    router.push('/')
  } else {
    console.error(result.error)
  }
}
</script>

<template>
  <div class="min-h-screen flex flex-col items-center justify-center">
    <form
      @submit.prevent="handleSubmit"
      class="flex flex-col gap-4 w-80 p-6 border rounded-lg shadow"
    >
      <input
        v-model="email"
        type="email"
        placeholder="Email"
        required
        class="border rounded px-3 py-2"
      />
      <input
        v-model="password"
        type="password"
        placeholder="Password"
        required
        class="border rounded px-3 py-2"
      />
      <button type="submit" class="bg-blue-500 text-white px-4 py-2 rounded">
        Sign In
      </button>
    </form>

    <p class="mt-4">
      Don't have an account?
      <NuxtLink to="/signup" class="text-blue-600">Sign up here</NuxtLink>.
    </p>
  </div>
</template>
