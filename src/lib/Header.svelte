<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();
</script>

<header class="header">
    <div class="header-content">
        <div class="logo-section">
            <h1 class="logo">Voice Assistant</h1>
        </div>

        <div class="right-section">
            <button class="help-button">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="icon">
                    <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm1 17h-2v-2h2v2zm2.07-7.75l-.9.92C13.45 12.9 13 13.5 13 15h-2v-.5c0-1.1.45-2.1 1.17-2.83l1.24-1.26c.37-.36.59-.86.59-1.41 0-1.1-.9-2-2-2s-2 .9-2 2H8c0-2.21 1.79-4 4-4s4 1.79 4 4c0 .88-.36 1.68-.93 2.25z"/>
                </svg>
                Help Center
            </button>
            <button class="sign-in">Sign In</button>
            <button class="sign-up">Sign Up</button>
        </div>
    </div>
</header>

<style>
    .header {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        background: rgba(255, 255, 255, 0.1);
        backdrop-filter: blur(10px);
        border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        z-index: 100;
    }

    .header-content {
        max-width: 1200px;
        margin: 0 auto;
        padding: 1rem 2rem;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .logo-section {
        display: flex;
        align-items: center;
    }

    .logo {
        font-size: 1.5rem;
        font-weight: 600;
        color: white;
        margin: 0;
    }

    .right-section {
        display: flex;
        align-items: center;
        gap: 1rem;
    }

    .help-button {
        display: flex;
        align-items: center;
        gap: 0.5rem;
        padding: 0.5rem 1rem;
        color: white;
        background: transparent;
        border: none;
        cursor: pointer;
        font-size: 0.9rem;
    }

    .icon {
        width: 20px;
        height: 20px;
    }

    .sign-in {
        padding: 0.5rem 1.5rem;
        color: white;
        background: transparent;
        border: 1px solid rgba(255, 255, 255, 0.2);
        border-radius: 6px;
        cursor: pointer;
        font-size: 0.9rem;
        transition: all 0.2s;
    }

    .sign-in:hover {
        background: rgba(255, 255, 255, 0.1);
    }

    .sign-up {
        padding: 0.5rem 1.5rem;
        color: white;
        background: #1d4ed8;
        border: none;
        border-radius: 6px;
        cursor: pointer;
        font-size: 0.9rem;
        transition: all 0.2s;
    }

    .sign-up:hover {
        background: #1e40af;
    }

    @media (max-width: 640px) {
        .header-content {
            padding: 1rem;
        }

        .help-button {
            display: none;
        }

        .sign-in, .sign-up {
            padding: 0.5rem 1rem;
            font-size: 0.8rem;
        }
    }
</style>
