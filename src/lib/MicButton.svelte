<script lang="ts">
    export let isActive: boolean;
    export let onClick: () => void;
</script>

<button
        class="mic-button {isActive ? 'active' : ''}"
        on:click={onClick}
        aria-label={isActive ? 'Stop Recording' : 'Start Recording'}
>
    <div class="button-inner">
        <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                fill="currentColor"
                class="mic-icon"
        >
            <path d="M12 15c1.66 0 3-1.34 3-3V6c0-1.66-1.34-3-3-3S9 4.34 9 6v6c0 1.66 1.34 3 3 3zm-1-9c0-.55.45-1 1-1s1 .45 1 1v6c0 .55-.45 1-1 1s-1-.45-1-1V6z"/>
            <path d="M17 11c0 2.76-2.24 5-5 5s-5-2.24-5-5H5c0 3.53 2.61 6.43 6 6.92V21h2v-3.08c3.39-.49 6-3.39 6-6.92h-2z"/>
        </svg>
    </div>

    {#if isActive}
        <div class="pulse-ring"></div>
        <div class="pulse-ring delay"></div>
    {/if}
</button>

<style>
    .mic-button {
        position: relative;
        width: 120px;
        height: 120px;
        border-radius: 50%;
        background: transparent;
        border: none;
        cursor: pointer;
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 0;
        transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    }

    .button-inner {
        position: relative;
        width: 80px;
        height: 80px;
        border-radius: 50%;
        background: linear-gradient(135deg, #6366f1, #a855f7);
        display: flex;
        align-items: center;
        justify-content: center;
        box-shadow: 0 8px 16px rgba(99, 102, 241, 0.2);
        transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    }

    .mic-button:hover .button-inner {
        transform: translateY(-2px);
        box-shadow: 0 12px 20px rgba(99, 102, 241, 0.3);
    }

    .mic-button:active .button-inner {
        transform: translateY(1px);
        box-shadow: 0 4px 8px rgba(99, 102, 241, 0.2);
    }

    .mic-icon {
        width: 36px;
        height: 36px;
        color: white;
        transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.1));
    }

    .active .button-inner {
        background: linear-gradient(135deg, #ef4444, #dc2626);
        box-shadow: 0 8px 16px rgba(239, 68, 68, 0.2);
    }

    .active .mic-icon {
        animation: pulse-icon 1s infinite;
    }

    .pulse-ring {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        width: 100%;
        height: 100%;
        border-radius: 50%;
        border: 2px solid rgba(239, 68, 68, 0.5);
        animation: pulse-ring 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
    }

    .pulse-ring.delay {
        animation-delay: 1s;
    }

    @keyframes pulse-ring {
        0% {
            width: 80px;
            height: 80px;
            opacity: 1;
            border-width: 2px;
        }
        100% {
            width: 120px;
            height: 120px;
            opacity: 0;
            border-width: 1px;
        }
    }

    @keyframes pulse-icon {
        0%, 100% {
            transform: scale(1);
        }
        50% {
            transform: scale(0.95);
        }
    }

    @media (max-width: 640px) {
        .mic-button {
            width: 100px;
            height: 100px;
        }

        .button-inner {
            width: 70px;
            height: 70px;
        }

        .mic-icon {
            width: 32px;
            height: 32px;
        }

        @keyframes pulse-ring {
            0% {
                width: 70px;
                height: 70px;
            }
            100% {
                width: 100px;
                height: 100px;
            }
        }
    }
</style>
