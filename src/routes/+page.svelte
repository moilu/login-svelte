<script lang="ts">
	import { goto } from '$app/navigation';
	import { form, field } from 'svelte-forms';
	import { required, email } from 'svelte-forms/validators';
	import '../app.css';

	let user = {
		email: 'asdasdsa@gmal.com',
		password: 'Password1'
	};

	const userEmail = field('email', '', [required(), email()]);
	const userPassword = field('password', '', [required()]);
	const loginForm = form(userEmail, userPassword);
	const passwordRegex = /^(?=.*[0-9!@#$%^&*(),.?":{}|<>])[0-9a-zA-Z!@#$%^&*(),.?":{}|<>]{3,}$/;

	$: validPassword = passwordRegex.test($userPassword.value);
	$: validForm = $loginForm.valid && validPassword;
	$: validCredentials = user.email === $userEmail.value && user.password === $userPassword.value;

	let warningMessage: string;

	function goToLogin(): void {
		if (validForm && validCredentials) {
			warningMessage = '';
			goto('/login');
		} else {
			$userPassword.value = '';
			$userEmail.value = '';
			warningMessage = 'Por favor verifica que tu correo y contraseña sean correctos.';
		}
	}
</script>

<section class="grid h-screen w-full place-items-center bg-gray-600 text-white">
	<form class="flex flex-col p-5 gap-y-5 md:p-10 " on:submit|preventDefault={goToLogin}>
		<label class="flex justify-between gap-5">
			<span>Correo</span>
			<input
				class="border-1-gray-400 rounded-md px-1 text-black"
				name="email"
				type="email"
				placeholder="correo@dominio.com"
				bind:value={$userEmail.value}
			/>
		</label>
		<label class="flex justify-between gap-5">
			<span>Contraseña</span>
			<input
				class="border-1-gray-400 rounded-md px-1 text-black"
				name="password"
				type="password"
				bind:value={$userPassword.value}
			/>
			<!-- <span class="font-bold hover:italic"
				>(Incluye minúsculas, mayúsculas y al menos un número o caracter especial.)</span
			> -->
		</label>
		<button
			class="py-2 px-4 bg-blue-500 text-white font-semibold rounded-md shadow-md focus:outline-none focus:ring-2 focus:ring-blue-400 focus:ring-opacity-75"
			disabled={!validForm}
			type="submit">Iniciar sesión</button
		>
		{#if warningMessage}
			<p>{warningMessage}</p>
		{/if}
	</form>
</section>
