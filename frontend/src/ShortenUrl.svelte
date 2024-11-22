<script>
    import { onMount } from "svelte";
    import { writable } from "svelte/store";
  
    let originalUrl = "";
    let shortenedUrl = writable("");
  
    async function shortenUrl() {
      if (originalUrl.trim() === "") {
        alert("Please enter a valid URL.");
        return;
      }
      
      try {
        const response = await fetch("https://url-shortener-hrk1.onrender.com/", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ url: originalUrl }),
        });
  
        if (response.ok) {
          const data = await response.json();
          shortenedUrl.set(data.shorten_url);
        } else {
          alert("Error: Unable to shorten the URL.");
        }
      } catch (error) {
        console.error("Error:", error);
        alert("Error: Could not reach the server.");
      }
    }
  </script>
  
  <style>
    /* Add basic styling */
    .container {
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-top: 50px;
    }
    .input-field {
      width: 300px;
      padding: 10px;
      font-size: 1em;
    }
    .button {
      padding: 10px 20px;
      margin-top: 10px;
      font-size: 1em;
    }
    .short-url {
      margin-top: 20px;
    }
  </style>
  
  <div class="container">
    <h1>URL Shortener</h1>
    <input
      type="text"
      class="input-field"
      placeholder="Enter URL"
      bind:value={originalUrl}
    />
    <button class="button" on:click={shortenUrl}>Shorten URL</button>
  
    {#if $shortenedUrl}
      <div class="short-url">
        <p>Shortened URL:</p>
        <a href="{$shortenedUrl}" target="_blank">{$shortenedUrl}</a>
      </div>
    {/if}
  </div>
  