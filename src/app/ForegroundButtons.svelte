<script lang="ts">
  import {nip19} from "nostr-tools"
  import {fade} from "svelte/transition"
  import user from "src/agent/user"
  import {modal, location} from "src/partials/state"

  let scrollY = 0

  $: showCreateNote = $location.pathname.match(/messages|home|chat|relays$|keys|settings|logout$/)
  $: showCreateTopicBtn = $location.pathname.match(/home/)

  const {canPublish} = user

  const scrollToTop = () => document.body.scrollIntoView({behavior: "smooth"})

  const createNote = () => {
    const pubkeyMatch = $location.pathname.match(/people\/(npub1[0-9a-z]+)/)
    const pubkey = pubkeyMatch ? nip19.decode(pubkeyMatch[1]).data : null
    const relayMatch = $location.pathname.match(/relays\/(.+)/)
    const relay = relayMatch ? relayMatch[1] : null
    const relays = relay ? [relay] : null

    modal.push({type: "note/create", pubkey, relays})
  }
  
  const createTopicBtn = () => {
    modal.push({type: "topicbtn/create"})
  }

  const deleteTopicBtn = () => {
    modal.push({type: "topicbtn/delete"})
  }

</script>

<svelte:window bind:scrollY />

<div class="fixed bottom-0 right-0 z-10 m-8 flex items-center gap-3">
  {#if scrollY > 1000}
    <button
      transition:fade|local={{delay: 200, duration: 200}}
      class="flex h-12 w-12 items-center justify-center rounded-full
          border border-gray-8 bg-gray-7 text-gray-1 shadow-2xl
          transition-all hover:scale-105 hover:bg-gray-6"
      on:click={scrollToTop}>
      <span class="fa fa-arrow-up" />
    </button>
  {/if}
  {#if $canPublish && !showCreateNote}
    <button
      class="color-white flex h-16 w-16 items-center justify-center rounded-full
            border border-accent-light bg-accent text-white shadow-2xl
            transition-all hover:scale-105 hover:bg-accent-light"
      on:click={createNote}>
      <span class="fa-plus fa-2xl" />
    </button>
  {/if}
  {#if $canPublish && showCreateTopicBtn}
    <button
      class="color-white flex h-16 w-16 items-center justify-center rounded-full
            border border-success-light bg-success text-white shadow-2xl
            transition-all hover:scale-105 hover:bg-success-light"
      on:click={createTopicBtn}>
      <i class="fa fa-plus" />
    </button>
    <button
      class="color-white flex h-16 w-16 items-center justify-center rounded-full
            border border-danger-light bg-danger text-white shadow-2xl
            transition-all hover:scale-105 hover:bg-danger-light"
      on:click={deleteTopicBtn}>
      <i class="fa fa-xmark" />
    </button>
  {/if}
</div>
