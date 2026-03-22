<script lang="ts">
    import { supabase } from "$lib/supabaseClient";
    import GloomIcon from "$lib/assets/gloom_icon.png";
    import PrimaryButton from "$lib/components/PrimaryButton.svelte";
    import InputGroup from "$lib/components/InputGroup.svelte";
	import SecondaryButton from "$lib/components/SecondaryButton.svelte";
    import InfoMessage from "$lib/components/InfoMessage.svelte";

    let joined = $state(false);
    let username = "";
    let email = $state("");
    let usernameError = $state("");
    let usernameErrorTimeout: ReturnType<typeof setTimeout>;
    let emailError = $state("");
    let emailErrorTimeout: ReturnType<typeof setTimeout>;
    let errorTimeoutDuration = 1500;

    let waitlistFormVisibility = $state("hidden");
    let showWaitlistBtnVisibility = $state("visible");

    function setUsernameError(msg: string) {
        usernameError = msg;
        clearTimeout(usernameErrorTimeout);
        usernameErrorTimeout = setTimeout(() => usernameError = "", errorTimeoutDuration);
    }

    function setEmailError(msg: string) {
        emailError = msg;
        clearTimeout(emailErrorTimeout);
        emailErrorTimeout = setTimeout(() => emailError = "", errorTimeoutDuration)
    }

    function validateUsername(username: string): string {
        /* RULES
            - Cant be empty
            - 3-20 characters
            - Only letters, numbers and underscores
            - Cannot start or end with an underscore 
        */

        if (!username) return "Username is required";
        if (username.length < 3) return "Must be at least 3 characters";
        if (username.length > 20) return "Must be under 20 characters";
        if (!/^[a-zA-Z0-9_]+$/.test(username)) return "Only letters, numbers and underscores.";
        if (/^_|_$/.test(username)) return "Cannot start or end with an underscore";
        return "";
    }

    function validateEmail(email: string): string {
        /* RULES
            - Cant be empty
            - Under 254 characters
            - In email format x@x.x
            - Cant start with "." or "@"
        */

        if (!email) return "Email is required";
        if (email.length > 254) return "Email is too long";
        if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) return "Invalid email address";
        if (email.includes("..")) return "Invalid email address";
        if (email.startsWith(".") || email.startsWith('@')) return "Invalid email address";
        return "";
    }

    async function join() {
        usernameError = validateUsername(username);
        if (usernameError) setUsernameError(usernameError);
        emailError = validateEmail(email);
        if (emailError) setEmailError(emailError);
        if (usernameError || emailError) return;

        const { error } = await supabase
            .from("waitlist")
            .insert({ username, email });

        if (error && error.code === "23505") {
            if (error.message.includes("username")) {
                setUsernameError("Username already taken");
            } else if (error.message.includes("email")) {
                setEmailError("Email already registered");
            }
        }

        if (!error) {
            joined = true;
        }
    }
</script>

<main>
    <div id="refresh-container">
        <button onclick={() => {window.location.reload();}}>
            <img src="{GloomIcon}" alt="">
            <p>GLOOM</p>
        </button>
    </div>

    <div id="content">
        <p id="header">GLOOM</p>
        <p id="description" class="secondary-text">
            Gloom brings privacy-first messaging, a fresh
            experience, and a design that actually puts users
            first, without the bloat.
        </p>
    </div>

    <PrimaryButton identifier="join-waitlist-btn"
        text="Join the Waitlist" fontSize="1.25rem" padding="12.5px 50px"
        onclick={() => {waitlistFormVisibility = "visible"; showWaitlistBtnVisibility = "hidden";}} style="visibility: {showWaitlistBtnVisibility}" />

    <div id="join-waitlist-form" style="visibility: {waitlistFormVisibility};">
        {#if joined}
            <InfoMessage identifier="waitlist-info-success"
                message="You're on the list!
                We'll email <b>{email}</b> when Gloom launches."
                borderColor="var(--accent-color)" borderDirection="left"
            />

            <SecondaryButton text="Close" fontSize="1.25rem" padding="10px 100px"
                onclick={() => {waitlistFormVisibility = "hidden"; showWaitlistBtnVisibility = "visible";}} />
        {:else}
            <div id="input-groups">
                <div class="input-wrapper">
                    <InputGroup bind:value={username} label="USERNAME" type="text" />
                    {#if usernameError}
                        <p class="error">{usernameError}</p>
                    {/if}
                </div>
                <div class="input-wrapper">
                    <InputGroup bind:value={email} label="EMAIL" type="email" />
                    {#if emailError}
                        <p class="error">{emailError}</p>
                    {/if}
                </div>
            </div>

            <InfoMessage identifier="waitlist-info"
                message="Your username will be reserved. You'll
                receive an email when Gloom launches
                to claim your account." 
                borderColor="var(--accent-color)" borderDirection="left"
            />

            <div id="button-group">
                <SecondaryButton text="Cancel" fontSize="1.25rem" padding="10px 100px"
                onclick={() => {waitlistFormVisibility = "hidden"; showWaitlistBtnVisibility = "visible";}} />
                <PrimaryButton identifier="sumbit-waitlist-btn" text="Join"
                    fontSize="1.25rem" padding="10px 100px" onclick={join} />
            </div>
        {/if}
    </div>

    <p class="secondary-text" id="copyright-notice">© 2026 Gloom. All rights reserved.</p>
</main>

<style>
    main {
        background-color: var(--bg-primary);
        font-family: var(--primary-font);
        
        position: fixed;
        top: 0;
        left: 0;
        width: 100svw;
        height: 100svh;

        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }

    #refresh-container {
        position: fixed;
        top: 0;
        left: 0;
        margin-left: 20px;
        margin-top: 20px;
    }
    
    #refresh-container img {
        width: 50px;
        height: 50px;
    }

    #refresh-container button {
        color: white;
        background: none;
        border: none;

        font-weight: 900;
        font-size: 1.25rem;
        text-decoration: none;

        display: flex;
        align-items: center;
        gap: 10px;
    } 

    #refresh-container button:hover {
        cursor: pointer;
    }

    #content {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }

    #header {
        background: linear-gradient(to bottom right, #FFFFFF 40%, var(--accent-color) 100%);
        -webkit-text-fill-color: transparent;
        -webkit-background-clip: text;

        font-weight: 900;
        font-style: italic;
        font-size: 6rem;
    }

    #header::after {
        content: "";
        padding: 2px;
    }

    .secondary-text {
        color: var(--secondary-text-color);
    }

    #description {
        font-size: 1.5rem;
        text-align: center;
        max-width: 600px;
    }

    :global(#join-waitlist-btn) {
        position: fixed;
        top: 75%;
    }

    #join-waitlist-form {
        visibility: hidden;
        background-color: var(--bg-secondary);
        border-radius: var(--border-radius);

        position: fixed;
        width: 600px;

        display: flex;
        flex-direction: column;
        gap: 40px;
        padding: 25px;
    }

    #input-groups {
        display: flex;
        flex-direction: column;
        gap: 40px;
    }

    .input-wrapper {
        display: flex;
        flex-direction: column;
        gap: 8px;
    }

    :global(#waitlist-info) {
        max-width: 420px;
    }

    #button-group {
        display: flex;
        justify-content: center;
        gap: 25px;
    }

    .error {
        color: rgb(255, 63, 63);
    }

    #copyright-notice {
        position: fixed;
        bottom: 0;
        left: 0;
        margin-left: 20px;
        margin-bottom: 20px;
    }
</style>