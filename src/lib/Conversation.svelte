<script>
    import {onDestroy} from 'svelte';
    import {Conversation} from '@11labs/client';

    // Reactive variables for UI state
    let connectionStatus = 'Disconnected';
    let agentStatus = 'Idle';
    let conversation = null;
    let conversationId = null;
    let showEmailForm = false;
    let email = '';
    let submitting = false;

    // Language selection state
    let selectedLanguage = 'en'; // Default language set to English

    // Define available languages
    const languages = [
        {id: 'en', name: 'English'},
        {id: 'hi', name: 'Hindi'}
    ];

    async function getSignedUrl() {
        const response = await fetch('https://uat-arise.gonuclei.com/elevenlabs/token');
        if (!response.ok) {
            throw new Error(`Failed to get signed url: ${response.statusText}`);
        }
        return await response.json();
    }

    // Function to start the conversation
    async function startConversation() {
        try {
            const data = await getSignedUrl();
            // Request microphone permission
            await navigator.mediaDevices.getUserMedia({audio: true});

            // Start the conversation session with the selected language
            conversation = await Conversation.startSession({
                signedUrl: data.signedUrl,
                agentId:  data.agentId,
                overrides: {
                    agent: {
                        language: selectedLanguage,  // Dynamically set language
                    },
                },
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
            conversationId = conversation.getId();
            showEmailForm = false;

            // Additional UI updates can be handled here if necessary
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
            showEmailForm = true;
        }
    }

    // Function to handle language changes
    async function handleLanguageChange(event) {
        const newLanguage = event.target.value;
        console.log("language change", {newLanguage}, {selectedLanguage})

        if (newLanguage !== selectedLanguage) {
            selectedLanguage = newLanguage;
            console.log("conversation 1", {conversation})
            if (conversation) {
                console.log("conversation", {conversation})
                // Restart the conversation with the new language
                await stopConversation();
                await startConversation();
            }
        }
    }

    async function handleEmailSubmit() {
        if (!email || !conversationId) return;

        submitting = true;
        try {
            const response = await fetch(`https://uat-arise.gonuclei.com/elevenlabs/conversation?conversation_id=${conversationId}&email=${encodeURIComponent(email)}`, {
                method: 'GET',
                headers: {
                    'Content-Type': 'application/json'
                }
            });

            if (!response.ok) {
                throw new Error('Failed to submit email');
            }

            // Clear form after successful submission
            email = '';
            showEmailForm = false;
        } catch (error) {
            console.error('Error submitting email:', error);
        } finally {
            submitting = false;
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

    /* Language Selector Styling */
    .language-selector {
        margin-bottom: 1.5rem;
    }

    select {
        padding: 0.5rem;
        font-size: 1rem;
        border-radius: 6px;
        border: 1px solid #ccc;
        width: 100%;
        max-width: 200px;
        margin: 0 auto;
        display: block;
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

    /* Email Form Styling */
    .email-form {
        margin-top: 2rem;
        padding: 1rem;
        border-top: 1px solid #eee;
    }

    .email-input {
        width: 100%;
        padding: 0.5rem;
        margin-bottom: 1rem;
        border: 1px solid #ccc;
        border-radius: 4px;
        font-size: 1rem;
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

        select {
            font-size: 0.9rem;
            padding: 0.4rem;
        }
    }
</style>

<div class="container">
    <!-- Language Selector -->
    <div class="language-selector">
        <select id="language" bind:value={selectedLanguage} on:change={handleLanguageChange}>
            {#each languages as lang}
                <option value={lang.id}>{lang.name}</option>
            {/each}
        </select>
    </div>

    <!-- Conversation Control Buttons -->
    <div class="buttons">
        <button on:click={startConversation} disabled={connectionStatus === 'Connected'}>
            Start Conversation
        </button>
        <button on:click={stopConversation} disabled={connectionStatus !== 'Connected'}>
            Stop Conversation
        </button>
    </div>

    <!-- Email Collection Form -->
    {#if showEmailForm}
        <div class="email-form">
            <h3>Please provide your email to get the conversation</h3>
            <input
                    type="email"
                    class="email-input"
                    placeholder="Enter your email"
                    bind:value={email}
                    disabled={submitting}
            />
            <button
                    on:click={handleEmailSubmit}
                    disabled={!email || submitting}
            >
                {submitting ? 'Submitting...' : 'Submit'}
            </button>
        </div>
    {/if}
</div>
