<script>
    import {onDestroy} from 'svelte';
    import {Conversation} from '@11labs/client';
    import Background from './Background.svelte';
    import MicButton from './MicButton.svelte';
    import Header from './Header.svelte';
    import LanguageSelector from './LanguageSelector.svelte';
    import Timer from './Timer.svelte';

    let isTimerActive = false;
    // Reactive variables for UI state
    let connectionStatus = 'Disconnected';
    let agentStatus = 'Idle';
    let conversation = null;
    let conversationId = null;
    let showEmailForm = false;
    let email = '';
    let submitting = false;
    let showControls = true;


    // Language selection state
    let selectedLanguage = 'en';

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

    async function toggleConversation() {
        if (connectionStatus === 'Connected') {
            await stopConversation();
        } else {
            await startConversation();
        }
    }

    async function startConversation() {
        try {
            const data = await getSignedUrl();
            await navigator.mediaDevices.getUserMedia({audio: true});

            conversation = await Conversation.startSession({
                signedUrl: data.signedUrl,
                agentId: data.agentId,
                overrides: {
                    agent: {
                        language: selectedLanguage,
                    },
                },
                onConnect: () => {
                    connectionStatus = 'Connected';
                    isTimerActive = true;
                },
                onDisconnect: () => {
                    connectionStatus = 'Disconnected';
                    isTimerActive = false;
                },
                onError: (error) => {
                    console.error('Error:', error);
                    isTimerActive = false;
                },
                onModeChange: (mode) => {
                    agentStatus = mode.mode === 'speaking' ? 'Speaking' : 'Listening';
                },
            });
            conversationId = conversation.getId();
            showEmailForm = false;
        } catch (error) {
            console.error('Failed to start conversation:', error);
            isTimerActive = false;
        }
    }

    async function handleLanguageChange(event) {
        const newLanguage = event.target.value;
        if (newLanguage !== selectedLanguage) {
            selectedLanguage = newLanguage;
            if (conversation) {
                await stopConversation();
                await startConversation();
            }
        }
    }

    function handleTimerTimeout() {
        stopConversation();
        showEmailForm = true;
    }


    async function stopConversation() {
        if (conversation) {
            await conversation.endSession();
            conversation = null;
            connectionStatus = 'Disconnected';
            agentStatus = 'Idle';
            showEmailForm = true;
            isTimerActive = false;
            showControls = false; // Hide controls when conversation stops
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

            email = '';
            showEmailForm = false;
            showControls = true; // Show controls after email submission
        } catch (error) {
            console.error('Error submitting email:', error);
        } finally {
            submitting = false;
        }
    }

    onDestroy(() => {
        if (conversation) {
            conversation.endSession();
        }
    });
</script>

<style>
    .main-content {
        padding-top: 5rem;
        min-height: 100vh;
        display: flex;
        flex-direction: column;
    }

    .hero {
        text-align: center;
        color: white;
        margin-bottom: 2rem;
        padding: 2rem;
    }

    .hero h1 {
        font-size: 2.5rem;
        font-weight: 700;
        margin-bottom: 1rem;
        background: linear-gradient(135deg, #6366f1, #a855f7);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
    }

    .hero p {
        font-size: 1.2rem;
        color: rgba(255, 255, 255, 0.8);
        max-width: 600px;
        margin: 0 auto;
    }

    .container {
        max-width: 400px;
        margin: 0 auto;
        padding: 2rem;
        border-radius: 12px;
        background: rgba(255, 255, 255, 0.1);
        backdrop-filter: blur(10px);
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        text-align: center;
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        border: 1px solid rgba(255, 255, 255, 0.1);
    }

    .language-selector {
        margin-bottom: 2rem;
    }

    select {
        padding: 0.5rem;
        font-size: 1rem;
        border-radius: 6px;
        border: 1px solid rgba(255, 255, 255, 0.2);
        width: 100%;
        max-width: 200px;
        margin: 0 auto;
        display: block;
        background: rgba(255, 255, 255, 0.1);
        color: white;
        backdrop-filter: blur(5px);
    }

    select option {
        background: #1e293b;
        color: white;
    }

    .mic-container {
        display: flex;
        flex-direction: column;
        align-items: center;
        margin: 2rem 0;
    }

    .email-form {
        margin-top: 2rem;
        padding: 1rem;
        border-top: 1px solid rgba(255, 255, 255, 0.1);
    }

    .email-form h3 {
        color: white;
        margin-bottom: 1rem;
    }

    .email-input {
        width: 100%;
        padding: 0.5rem;
        margin-bottom: 1rem;
        border: 1px solid rgba(255, 255, 255, 0.2);
        border-radius: 4px;
        font-size: 1rem;
        background: rgba(255, 255, 255, 0.1);
        color: white;
        backdrop-filter: blur(5px);
    }

    .email-input::placeholder {
        color: rgba(255, 255, 255, 0.5);
    }

    .submit-button {
        padding: 0.75rem 1rem;
        font-size: 1rem;
        font-weight: 600;
        color: white;
        background: linear-gradient(135deg, rgba(99, 102, 241, 0.8), rgba(168, 85, 247, 0.8));
        border: none;
        border-radius: 8px;
        cursor: pointer;
        backdrop-filter: blur(5px);
        transition: transform 0.2s ease, box-shadow 0.2s ease;
    }

    .submit-button:hover:not(:disabled) {
        transform: translateY(-2px);
        box-shadow: 0 6px 8px rgba(0, 0, 0, 0.15);
    }

    .submit-button:disabled {
        background: rgba(156, 163, 175, 0.5);
        cursor: not-allowed;
    }

    @media (max-width: 500px) {
        .hero h1 {
            font-size: 2rem;
        }

        .hero p {
            font-size: 1rem;
        }

        .container {
            padding: 1.5rem;
            margin: 1rem;
        }

        select {
            font-size: 0.9rem;
            padding: 0.4rem;
        }
    }
</style>

<Background/>
<Header/>

<div class="main-content">
    <div class="hero">
        <p>Start a conversation with our AI-powered voice assistant</p>
    </div>

    <div class="container">
        {#if showControls}
            <LanguageSelector
                    selectedLanguage={selectedLanguage}
                    onLanguageChange={handleLanguageChange}
            />

            <div class="mic-container">
                <MicButton
                        isActive={connectionStatus === 'Connected'}
                        onClick={toggleConversation}
                />
                <Timer
                        isActive={isTimerActive}
                        onTimeout={handleTimerTimeout}
                />
            </div>
        {/if}

        {#if showEmailForm}
            <div class="email-form">
                <h3>Please provide your email</h3>
                <input
                        type="email"
                        class="email-input"
                        placeholder="Enter your email"
                        bind:value={email}
                        disabled={submitting}
                />
                <button
                        class="submit-button"
                        on:click={handleEmailSubmit}
                        disabled={!email || submitting}
                >
                    {submitting ? 'Submitting...' : 'Submit'}
                </button>
            </div>
        {/if}
    </div>
</div>
