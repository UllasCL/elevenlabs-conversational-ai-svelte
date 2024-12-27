<script lang="ts">
    import {onDestroy} from 'svelte';

    export let duration = 120; // 2 minutes in seconds
    export let isActive = false;
    export let onTimeout: () => void;

    let timeLeft = duration;
    let intervalId: number;

    $: minutes = Math.floor(timeLeft / 60);
    $: seconds = timeLeft % 60;

    $: if (isActive && !intervalId) {
        startTimer();
    } else if (!isActive && intervalId) {
        stopTimer();
    }

    function startTimer() {
        timeLeft = duration;
        intervalId = setInterval(() => {
            timeLeft -= 1;
            if (timeLeft <= 0) {
                stopTimer();
                onTimeout();
            }
        }, 1000);
    }

    function stopTimer() {
        if (intervalId) {
            clearInterval(intervalId);
            intervalId = undefined;
        }
    }

    onDestroy(() => {
        stopTimer();
    });
</script>

<div class="timer">
    {minutes}:{seconds.toString().padStart(2, '0')}
</div>

<style>
    .timer {
        font-size: 1.5rem;
        font-weight: bold;
        color: white;
        margin-top: 1rem;
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }
</style>
