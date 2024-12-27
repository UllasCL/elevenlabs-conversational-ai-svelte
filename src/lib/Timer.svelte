<script lang="ts">
    import { onDestroy, createEventDispatcher } from 'svelte';

    export let isActive: boolean = false;
    export let duration: number = 120; // 2 minutes in seconds

    const dispatch = createEventDispatcher();
    let timeLeft = duration;
    let interval: number;

    $: if (isActive && !interval) {
        startTimer();
    } else if (!isActive && interval) {
        stopTimer();
    }

    function startTimer() {
        timeLeft = duration;
        interval = setInterval(() => {
            timeLeft--;
            if (timeLeft <= 0) {
                stopTimer();
                dispatch('timeout');
            }
        }, 1000);
    }

    function stopTimer() {
        if (interval) {
            clearInterval(interval);
            interval = undefined;
        }
    }

    onDestroy(() => {
        stopTimer();
    });
</script>

{#if isActive}
    <div class="timer">
        <div class="time">
            {Math.floor(timeLeft / 60)}:{(timeLeft % 60).toString().padStart(2, '0')}
        </div>
        <div class="progress-ring">
            <svg viewBox="0 0 36 36">
                <path
                        d="M18 2.0845
            a 15.9155 15.9155 0 0 1 0 31.831
            a 15.9155 15.9155 0 0 1 0 -31.831"
                        fill="none"
                        stroke="rgba(255, 255, 255, 0.2)"
                        stroke-width="2"
                />
                <path
                        d="M18 2.0845
            a 15.9155 15.9155 0 0 1 0 31.831
            a 15.9155 15.9155 0 0 1 0 -31.831"
                        fill="none"
                        stroke="#6366f1"
                        stroke-width="2"
                        stroke-dasharray={`${(timeLeft / duration) * 100}, 100`}
                />
            </svg>
        </div>
    </div>
{/if}

<style>
    .timer {
        position: absolute;
        top: -40px;
        left: 50%;
        transform: translateX(-50%);
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .time {
        font-size: 0.875rem;
        font-weight: 500;
        color: white;
        margin-bottom: 0.25rem;
    }

    .progress-ring {
        position: absolute;
        width: 36px;
        height: 36px;
    }

    svg {
        transform: rotate(-90deg);
    }

    path {
        transition: stroke-dasharray 0.3s ease;
    }
</style>
