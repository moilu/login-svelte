<script lang="ts">
	import { goto } from '$app/navigation';
    import { form, field } from 'svelte-forms';
    import { required, email } from 'svelte-forms/validators';
    // import "../app.css";

    let user = {
        email: 'asdasdsa@gmal.com',
        password: 'Password1'
    };

    // Investigar goTo en routing de sveltekit, poner errores de input(vacio, credenciales incorrectas);
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

<form on:submit|preventDefault={goToLogin}>
    <label class="form-text-input">
        <span class="label">Correo</span>
        <input  name="email" type="email" 
            placeholder="correo@dominio.com"
            bind:value={$userEmail.value}>
    </label>
    <label class="form-text-input">
        <span class="label">Contraseña</span>
        <input name="password" type="password" 
            bind:value={$userPassword.value}>    
        <span>(Incluye minúsculas, mayúsculas y al menos un número o caracter especial.)</span> 
    </label>
    {#if warningMessage}
        <p>{warningMessage}</p>
    {/if}
    <button disabled={!validForm} type="submit">Iniciar sesión</button>
</form>