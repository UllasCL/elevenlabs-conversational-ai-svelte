<script lang="ts">
    export let mode: 'idle' | 'user' | 'bot' = 'idle';

    $: gradientClass = {
        idle: 'gradient-idle',
        user: 'gradient-user',
        bot: 'gradient-bot'
    }[mode];
</script>

<div class="background {gradientClass}">
    <div class="circles">
        {#each Array(10) as _, i}
            <div class="circle" style="--i:{i}"/>
        {/each}
    </div>
</div>

<style>
    .background {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: -1;
        overflow: hidden;
        transition: background 0.5s ease;
    }

    .gradient-idle {
        background: linear-gradient(45deg, #1a1a1a, #2d2d2d);
    }

    .gradient-user {
        background: linear-gradient(45deg, #1e3a8a, #3b82f6);
    }

    .gradient-bot {
        background: linear-gradient(45deg, #5b21b6, #8b5cf6);
    }

    .circles {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        overflow: hidden;
    }

    .circle {
        position: absolute;
        border-radius: 50%;
        background: rgba(255, 255, 255, 0.1);
        animation: float 20s linear infinite;
        animation-delay: calc(var(--i) * -2s);
    }

    .circle:nth-child(odd) {
        width: 100px;
        height: 100px;
    }

    .circle:nth-child(even) {
        width: 150px;
        height: 150px;
    }

    @keyframes float {
        0% {
            transform: translateY(100vh) translateX(100vw) scale(0);
            opacity: 0;
        }
        50% {
            opacity: 0.5;
        }
        100% {
            transform: translateY(-100vh) translateX(-100vw) scale(1);
            opacity: 0;
        }
    }
</style>
