<script>
    let username = '';
    let user = null;

    async function fetchUser() {
        if (username.trim() === '') {
            user = null;
            return;
        }
        const response = await fetch(`https://api.github.com/users/${username}`);
        if (response.ok) {
            user = await response.json();
        } else {
            user = null;
        }
    }
</script>

<style>
    @import url('/global.css');
    .avatar {
        border-radius: 50%;
        width: 100px;
        height: 100px;
    }
</style>

<header class="bg-blue-600 text-white p-4 flex justify-between items-center">
    <h1 class="text-3xl">GitHub Search App</h1>
    <button on:click={() => document.documentElement.classList.toggle('dark')} class="bg-gray-700 text-white p-2 rounded">Toggle Theme</button>
</header>

<nav class="bg-gray-800 text-white p-4">
    <ul class="flex space-x-4">
        <li><a href="#" class="hover:underline">Inicio</a></li>
        <li><a href="#" class="hover:underline">Sobre Nosotros</a></li>
        <li><a href="#" class="hover:underline">Contacto</a></li>
    </ul>
</nav>

<main class="p-4 max-w-screen-lg mx-auto">
    <section>
        <h2 class="text-2xl mb-4">Buscar Usuario de GitHub</h2>
        <form on:submit|preventDefault={fetchUser} class="mb-4 flex flex-col sm:flex-row items-stretch">
            <input type="text" bind:value={username} class="border p-2 w-full sm:w-auto flex-grow" placeholder="Ingrese nombre de usuario">
            <button type="submit" class="bg-blue-500 text-white p-2 mt-2 sm:mt-0 sm:ml-2">Buscar</button>
        </form>
        <div id="result" class="mt-4">
            {#if user}
                <div class="user-info p-4 bg-gray-100 dark:bg-gray-800 rounded-lg shadow-md">
                    <img src={user.avatar_url} alt={user.login} class="avatar mx-auto">
                    <h2 class="text-2xl text-center mt-4">{user.login}</h2>
                    <p class="text-center mt-2">{user.bio ? user.bio : 'No bio available'}</p>
                    <ul class="mt-4">
                        <li><strong>Repos:</strong> {user.public_repos}</li>
                        <li><strong>Followers:</strong> {user.followers}</li>
                        <li><strong>Following:</strong> {user.following}</li>
                    </ul>
                </div>
            {:else if username.trim() !== ''}
                <p>No se encontró ningún usuario.</p>
            {/if}
        </div>
    </section>
</main>

<footer class="bg-gray-800 text-white p-4 text-center">
    &copy; 2024 GitHub Search App. Todos los derechos reservados.
</footer>
