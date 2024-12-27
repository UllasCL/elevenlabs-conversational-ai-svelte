<script lang="ts">
    export let selectedLanguage: string;
    export let onLanguageChange: (event: Event) => void;

    const languages = [
        { id: 'en', name: 'English', flag: '🇺🇸' },
        { id: 'hi', name: 'Hindi', flag: '🇮🇳' }
    ];
</script>

<div class="language-selector">
    <div class="select-wrapper">
        <select
                value={selectedLanguage}
                on:change={onLanguageChange}
                class="select-input"
        >
            {#each languages as lang}
                <option value={lang.id}>
                    {lang.flag} {lang.name}
                </option>
            {/each}
        </select>
        <svg class="select-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
            <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd" />
        </svg>
    </div>
</div>

<style>
    .language-selector {
        margin-bottom: 2rem;
    }

    .select-wrapper {
        position: relative;
        max-width: 200px;
        margin: 0 auto;
    }

    .select-input {
        appearance: none;
        width: 100%;
        padding: 0.75rem 1rem;
        padding-right: 2.5rem;
        font-size: 1rem;
        line-height: 1.5;
        border-radius: 8px;
        border: 1px solid rgba(255, 255, 255, 0.2);
        background: rgba(255, 255, 255, 0.1);
        color: white;
        cursor: pointer;
        backdrop-filter: blur(10px);
        transition: all 0.2s ease;
    }

    .select-input:hover {
        border-color: rgba(255, 255, 255, 0.3);
        background: rgba(255, 255, 255, 0.15);
    }

    .select-input:focus {
        outline: none;
        border-color: rgba(99, 102, 241, 0.5);
        box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.25);
    }

    .select-icon {
        position: absolute;
        right: 0.75rem;
        top: 50%;
        transform: translateY(-50%);
        width: 1.25rem;
        height: 1.25rem;
        color: rgba(255, 255, 255, 0.7);
        pointer-events: none;
    }

    .select-input option {
        background: #1e293b;
        color: white;
        padding: 0.5rem;
    }

    @media (max-width: 640px) {
        .select-input {
            padding: 0.6rem 0.8rem;
            font-size: 0.9rem;
        }
    }
</style>
