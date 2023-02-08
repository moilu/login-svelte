<script lang="ts">
  import { form, field } from 'svelte-forms';
  import { required, email } from 'svelte-forms/validators';

    const userEmail = field('email', '', [required(), email()]);
    const userPassword = field('password', '', [required()]);
    const loginForm = form(userEmail, userPassword);
    const passwordRegex = /^(?=.*[0-9!@#$%^&*(),.?":{}|<>])[0-9a-zA-Z!@#$%^&*(),.?":{}|<>]{8,}$/;

    $: validPassword = passwordRegex.test($userPassword.value);
    $: validForm = $loginForm.valid && validPassword;

    function goToLogin(): void {
        if (validForm) {
            window.location.assign('/login');
        }
    }

</script>

<form on:submit|preventDefault={goToLogin}>
    <label class="form-text-input">
        <span class="label">Correo</span>
        <input name="email" type="email" 
            placeholder="correo@dominio.com"
            bind:value={$userEmail.value}>
    </label>
    <label class="form-text-input">
        <span class="label">Contraseña</span>
        <input name="password" type="password" 
            placeholder=""
            bind:value={$userPassword.value}>    
        <span>Asegúrate de que la contraseña incluya letras minúsculas, mayúsculas y al menos un número o caracter especial.</span> 
    </label>
    <button disabled={!validForm} type="submit">Iniciar sesión</button>
</form>