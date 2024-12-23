<script>
    import { onDestroy } from 'svelte';
    import { Conversation } from '@11labs/client';

    // Reactive variables for UI state
    let connectionStatus = 'Disconnected';
    let agentStatus = 'Idle';
    let conversation = null;

    // Function to start the conversation
    async function startConversation() {
        try {
            // Request microphone permission
            await navigator.mediaDevices.getUserMedia({ audio: true });

            // Start the conversation session
            conversation = await Conversation.startSession({
                agentId: 'cwpy6HtdtlwBCNspFhvB', // Replace with your agent ID
                onConnect: () => {
                    connectionStatus = 'Connected';
                },
                onDisconnect: () => {
                    connectionStatus = 'Disconnected';
                },
                onError: (error) => {
                    console.error('Error:', error);
                },
                onModeChange: (mode) => {
                    agentStatus = mode.mode === 'speaking' ? 'Speaking' : 'Listening';
                },
            });

            // Update UI state after successful connection
        } catch (error) {
            console.error('Failed to start conversation:', error);
        }
    }

    // Function to stop the conversation
    async function stopConversation() {
        if (conversation) {
            await conversation.endSession();
            conversation = null;
            connectionStatus = 'Disconnected';
            agentStatus = 'Idle';
        }
    }

    // Cleanup on component destroy
    onDestroy(() => {
        if (conversation) {
            conversation.endSession();
        }
    });
</script>

<style>
    /* Container Styling */
    .container {
        max-width: 400px;
        margin: 2rem auto;
        padding: 2rem;
        border-radius: 12px;
        background: #f9f9f9;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        text-align: center;
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }

    /* Status Display Styling */
    .status {
        margin-bottom: 1.5rem;
        font-size: 1.1rem;
    }

    .status strong {
        color: #333;
    }

    /* Button Styling */
    .buttons {
        display: flex;
        justify-content: center;
        gap: 1rem;
    }

    button {
        flex: 1;
        padding: 0.75rem 1rem;
        font-size: 1rem;
        font-weight: 600;
        color: #fff;
        background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
        border: none;
        border-radius: 8px;
        cursor: pointer;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        transition: transform 0.2s ease, box-shadow 0.2s ease;
    }

    button:hover:not(:disabled) {
        transform: translateY(-2px);
        box-shadow: 0 6px 8px rgba(0, 0, 0, 0.15);
    }

    button:active:not(:disabled) {
        transform: translateY(0);
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }

    button:disabled {
        background: #ccc;
        cursor: not-allowed;
        box-shadow: none;
        transform: none;
    }

    /* Responsive Design */
    @media (max-width: 500px) {
        .container {
            padding: 1.5rem;
        }

        button {
            font-size: 0.9rem;
            padding: 0.6rem 0.8rem;
        }

        .status {
            font-size: 1rem;
        }
    }
</style>

<div class="container">
    <div class="buttons">
        <button on:click={startConversation} disabled={connectionStatus === 'Connected'}>
            Start Conversation
        </button>
        <button on:click={stopConversation} disabled={connectionStatus !== 'Connected'}>
            Stop Conversation
        </button>
    </div>
</div>
