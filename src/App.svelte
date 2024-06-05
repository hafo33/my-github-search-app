<script>
    import { onMount } from 'svelte';

    let username = '';
    let user = null;
    let theme = 'dark'; // Inicializar con el tema oscuro

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

    function toggleTheme() {
        theme = theme === 'light' ? 'dark' : 'light';
        document.documentElement.classList.toggle('dark', theme === 'dark');
    }

    onMount(() => {
        document.documentElement.classList.add('dark');
    });
</script>

<style>
    @import url('/global.css');
    @import url('https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&display=swap');
    @import url('https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css');

    :global(:root) {
        --bg-color-light: #f6f8ff;
        --bg-color-dark: #141d2f;
        --text-color-light: #4b6a9b;
        --text-color-dark: #ffffff;
        --card-bg-light: #fefefe;
        --card-bg-dark: #1e2a47;
        --search-bg-light: #fefefe;
        --search-bg-dark: #1e2a47;
        --button-bg: #0079ff;
        --button-text: #ffffff;
        --stats-bg-dark: #222731;
        --stats-bg-light: #f1f1f1a8;
        --stats-text-color-light: #4b6a9b;
        --stats-text-color-dark: #ffffffa1;
        --info-bg-dark: #2b3442;
    }

    :global(html) {
        font-family: 'Space Mono', monospace;
        transition: background-color 0.3s, color 0.3s;
    }

    :global(body) {
        margin: 0;
        padding: 0;
    }

    :global(.light) {
        --bg-color: var(--bg-color-light);
        --text-color: var(--text-color-light);
        --card-bg: var(--card-bg-light);
        --search-bg: var(--search-bg-light);
        --button-bg: var(--button-bg);
        --button-text: var(--button-text);
        --stats-bg: var(--stats-bg-light);
        --stats-text-color: var(--stats-text-color-light);
    }

    :global(.dark) {
        --bg-color: var(--bg-color-dark);
        --text-color: var(--text-color-dark);
        --card-bg: var(--card-bg-dark);
        --search-bg: var(--search-bg-dark);
        --button-bg: var(--button-bg);
        --button-text: var(--button-text);
        --stats-bg: var(--stats-bg-dark);
        --stats-text-color: var(--stats-text-color-dark);
    }

    :global(html.light) {
        background-color: var(--bg-color);
        color: var(--text-color);
    }

    :global(html.dark) {
        background-color: var(--bg-color);
        color: var(--text-color);
    }

    .container {
        max-width: 600px;
        margin: 0 auto;
        padding: 0.5rem; /* Reducir padding */
    }

    header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 0.5rem 0; /* Reducir padding */
    }

    .title {
        text-transform: lowercase;
        font-size: 1.5rem;
        font-weight: bold;
        color: var(--text-color);
    }

    .theme-toggle {
        cursor: pointer;
        background: none;
        border: none;
        color: var(--text-color);
        font-weight: bold;
        text-transform: uppercase;
    }

    .search-container {
        display: flex;
        justify-content: center;
        padding: 0.5rem 0; /* Reducir padding */
    }

    .search {
        display: flex;
        align-items: center;
        background-color: var(--search-bg);
        border-radius: 10px; /* Ajuste de redondeo */
        padding: 0.5rem; /* Reducir padding */
        width: 100%;
        box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2); /* Sombra */
    }

    .search input {
        flex-grow: 1;
        border: none;
        outline: none;
        background-color: transparent;
        color: var(--text-color);
        padding: 0.5rem;
        font-family: 'Space Mono', monospace;
        font-size: 1rem; /* Ajuste de tamaño de fuente */
    }

    .search button {
        background-color: #0079ff; /* Color azul */
        color: #ffffff; /* Texto en blanco */
        border: none;
        padding: 0.5rem 1rem;
        border-radius: 5px;
        cursor: pointer;
        font-family: 'Space Mono', monospace;
        font-size: 1rem;
    }

    .card {
        background-color: var(--card-bg);
        border-radius: 10px;
        padding: 1rem; /* Reducir padding */
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        margin: 0.5rem 0; /* Reducir margen */
        color: var(--text-color);
    }

    .card-header {
        display: flex;
        justify-content: space-between; /* Ajuste para alinear los elementos correctamente */
        align-items: center;
        margin-bottom: 1rem;
    }

    .avatar {
        border-radius: 50%;
        width: 100px;
        height: 100px;
        margin-right: 1rem;
    }

    .user-info {
        flex: 1;
    }

    .user-info h2 {
        margin: 0;
        font-size: 1.25rem;
    }

    .user-info p {
        margin: 0.25rem 0;
    }

    .stats {
        display: flex;
        justify-content: space-around;
        background-color: var(--stats-bg); /* Fondo más oscuro */
        padding: 0.5rem; /* Reducir padding */
        border-radius: 10px;
        margin-top: 0.5rem; /* Reducir margen */
        color: var(--stats-text-color); /* Usar variable de color de texto de estadísticas */
    }

    .info {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 0.5rem;
        margin-top: 0.5rem; /* Reducir margen */
    }

    .info-item {
        display: flex;
        align-items: center;
        gap: 0.5rem;
    }

    .stat-item {
        text-align: center;
    }

    .stat-item h3 {
        font-size: 1rem;
        margin-bottom: 0.5rem;
        text-transform: uppercase;
    }

    .stat-item p {
        font-size: 1.5rem; /* Ajuste de tamaño de fuente */
        font-weight: bold;
        margin: 0;
    }

    .icon {
        font-size: 1.2rem;
    }
</style>

<div class="container">
    <header>
        <h1 class="title">devfinder</h1>
        <button on:click={toggleTheme} class="theme-toggle">
            {theme === 'light' ? 'LIGHT' : 'DARK'}
        </button>
    </header>

    <main>
        <section class="search-container">
            <div class="search">
                <input type="text" bind:value={username} placeholder="🔎 Search GitHub username…" />
                <button on:click={fetchUser}>Search</button>
            </div>
        </section>

        {#if user}
            <article class="card">
                <header class="card-header">
                    <img src={user.avatar_url} alt={user.login} class="avatar">
                    <div class="user-info">
                        <h2>{user.name}</h2>
                        <p>@{user.login}</p>
                        <p>Joined {new Date(user.created_at).toDateString()}</p>
                    </div>
                </header>
                <p>{user.bio ? user.bio : 'This profile has no bio'}</p>
                <section class="stats">
                    <div class="stat-item">
                        <h3>Repos</h3>
                        <p>{user.public_repos}</p>
                    </div>
                    <div class="stat-item">
                        <h3>Followers</h3>
                        <p>{user.followers}</p>
                    </div>
                    <div class="stat-item">
                        <h3>Following</h3>
                        <p>{user.following}</p>
                    </div>
                </section>
                <section class="info">
                    <div class="info-item">
                        <i class="fas fa-map-marker-alt icon"></i>
                        <p>{user.location ? user.location : 'Not Available'}</p>
                    </div>
                    <div class="info-item">
                        <i class="fas fa-link icon"></i>
                        <p>{user.blog ? `<a href="${user.blog}" target="_blank">${user.blog}</a>` : 'Not Available'}</p>
                    </div>
                    <div class="info-item">
                        <i class="fab fa-twitter icon"></i>
                        <p>{user.twitter_username ? `@${user.twitter_username}` : 'Not Available'}</p>
                    </div>
                    <div class="info-item">
                        <i class="fas fa-building icon"></i>
                        <p>{user.company ? user.company : 'Not Available'}</p>
                    </div>
                </section>
            </article>
        {/if}
    </main>
</div>
