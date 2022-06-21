<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { 
	useSignInEmailPassword,
	useSignUpEmailPassword
} from '@nhost/vue'

const router = useRouter()

const isRegister = ref(false)

const email = ref('')
const password = ref('')

const { signUpEmailPassword } = useSignUpEmailPassword()
const { signInEmailPassword } = useSignInEmailPassword()

const registerOrLogin = async () => {
	if (
		!email.value || 
		!password.value
	) return alert("Please fill in all fields")

	const res = isRegister.value ? await signUpEmailPassword(email.value, password.value) : await signInEmailPassword(email.value, password.value)

	if (res.isError) return alert(res.error.message)

	router.push('/')
}
</script>

<template>
	<main>
		<h1 class="text-4xl text-gray-400 font-bold mb-8">Notes Nhost</h1>
		
		<form @submit.prevent="registerOrLogin">
			<h3 class="text-xl text-gray-400 font-bold uppercase mb-4">{{ isRegister ? 'Register' : 'Login' }} Form</h3>

			<label class="block mb-4">
				<span class="block text-gray-400 text-sm uppercase mb-2">Email</span>
				<input type="email" v-model="email" class="block w-full text-slate-800 px-4 py-2" />
			</label>

			<label class="block mb-4">
				<span class="block text-gray-400 text-sm uppercase mb-2">Password</span>
				<input type="password" v-model="password" class="block w-full text-slate-800 px-4 py-2" />
			</label>

			<input type="submit" :value="isRegister ? 'Register' : 'Login'" class="text-gray-100 hover:underline text-lg mb-4 cursor-pointer" />

			<p class="text-gray-400">
				Go to <button type="button" @click="() => isRegister = !isRegister" class="text-gray-100 text-lg mt-2 hover:underline">{{ isRegister ? 'Login Form' : 'Register Form' }}</button> ?
			</p>
		</form>
	</main>
</template>