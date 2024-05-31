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
    .card {
        background-color: var(--card-bg);
        border-radius: 10px;
        padding: 2rem;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }
    .search {
        display: flex;
        align-items: center;
        background-color: var(--search-bg);
        border-radius: 5px;
        padding: 0.5rem;
        margin-bottom: 1rem;
    }
    .search input {
        flex-grow: 1;
        border: none;
        outline: none;
        background-color: transparent;
        color: var(--text-color);
        padding: 0.5rem;
    }
    .search button {
        background-color: var(--button-bg);
        color: var(--button-text);
        border: none;
        padding: 0.5rem 1rem;
        border-radius: 5px;
        cursor: pointer;
    }
</style>

<header class="bg-blue-600 text-white p-4 flex justify-between items-center">
    <h1 class="text-3xl">devfinder</h1>
    <button on:click={() => document.documentElement.classList.toggle('dark')} class="bg-gray-700 text-white p-2 rounded">Light</button>
</header>

<main class="p-4 max-w-screen-lg mx-auto">
    <section class="search">
        <input type="text" bind:value={username} placeholder="Search GitHub username…" />
        <button on:click={fetchUser}>Search</button>
    </section>
    <section>
        {#if user}
            <div class="card">
                <div class="flex items-center mb-4">
                    <img src={user.avatar_url} alt={user.login} class="avatar mr-4">
                    <div>
                        <h2 class="text-2xl">{user.name}</h2>
                        <p>@{user.login}</p>
                        <p>Joined {new Date(user.created_at).toDateString()}</p>
                    </div>
                </div>
                <p class="mb-4">{user.bio ? user.bio : 'This profile has no bio'}</p>
                <div class="flex justify-between bg-gray-100 p-4 rounded-lg">
                    <div>
                        <h3>Repos</h3>
                        <p>{user.public_repos}</p>
                    </div>
                    <div>
                        <h3>Followers</h3>
                        <p>{user.followers}</p>
                    </div>
                    <div>
                        <h3>Following</h3>
                        <p>{user.following}</p>
                    </div>
                </div>
                <div class="mt-4">
                    <p><i class="fas fa-map-marker-alt"></i> {user.location ? user.location : 'Not Available'}</p>
                    <p><i class="fas fa-link"></i> {user.blog ? user.blog : 'Not Available'}</p>
                    <p><i class="fab fa-twitter"></i> {user.twitter_username ? `@${user.twitter_username}` : 'Not Available'}</p>
                    <p><i class="fas fa-building"></i> {user.company ? user.company : 'Not Available'}</p>
                </div>
            </div>
        {:else if username.trim() !== ''}
            <p>No se encontró ningún usuario.</p>
        {/if}
    </section>
</main>
