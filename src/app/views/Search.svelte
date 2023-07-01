<script>
  import {debounce} from "throttle-debounce"
  import {navigate} from "svelte-routing"
  import {nip05, nip19} from "nostr-tools"
  import {identity} from "ramda"
  import {fuzzy, tryFunc} from "src/util/misc"
  import {fromNostrURI} from "src/util/nostr"
  import {modal} from "src/partials/state"
  import Input from "src/partials/Input.svelte"
  import Heading from "src/partials/Heading.svelte"
  import Content from "src/partials/Content.svelte"
  import BorderLeft from "src/partials/BorderLeft.svelte"
  import Scan from "src/app/shared/Scan.svelte"
  import PersonInfo from "src/app/shared/PersonInfo.svelte"
  import {sampleRelays, getUserReadRelays} from "src/agent/relays"
  import network from "src/agent/network"
  import {watch, people} from "src/agent/db"
  import user from "src/agent/user"
  import Feed from "../shared/Feed.svelte"

  let q = ""
  let scanner

  export let searchkey;
  if(searchkey) q = searchkey;
  
  const tryParseEntity = debounce(500, async entity => {
    entity = fromNostrURI(entity)

    if (entity.length < 5) {
      return
    }

    if (entity.match(/^[a-f0-9]{64}$/)) {
      navigate("/" + nip19.npubEncode(entity))
    } else if (entity.includes("@")) {
      let profile = await nip05.queryProfile(entity)

      if (profile) {
        navigate("/" + nip19.nprofileEncode(profile))
      }
    } else {
      tryFunc(() => {
        nip19.decode(entity)
        navigate("/" + entity)
      }, "TypeError")
    }
  })

  $: {
    tryParseEntity(q)
  }

  let relays = sampleRelays(getUserReadRelays())
  let filter = {kinds: [1], search: q}
  function searchNotes(ev) {
    if (q.length > 2) {
      relays = sampleRelays(getUserReadRelays())
      filter = {kinds: [1], search: q}
    }
    if (q.length == 0) {
      relays = sampleRelays(getUserReadRelays())
      filter = {kinds: [1], search: q}
    }
  }

  document.title = "Search"
</script>

<Content>
  <!-- <div class="flex flex-col items-center justify-center">
    <Heading>Search</Heading>
    <p>
      Enter any nostr identifier or search term to find people and topics. You can also click on the
      camera icon to scan with your device's camera instead.
    </p>
  </div> -->
  <Input autofocus bind:value={q} on:input={(ev) => searchNotes(ev)}>
    <i slot="before" class="fa-solid fa-search" />
    <i
      slot="after"
      class="fa-solid fa-camera cursor-pointer text-accent"
      on:click={() => scanner.start()} />
  </Input>
  <!-- <Input bind:value={q} placeholder="Search for people or topics">
    <i slot="before" class="fa-solid fa-search" />
    <i
      slot="after"
      class="fa-solid fa-camera cursor-pointer text-accent"
      on:click={() => scanner.start()} />
  </Input> -->

  <!-- {#each search(q).slice(0, 50) as result (result.type + result.id)}
    {#if result.type === "topic"}
      <BorderLeft on:click={() => openTopic(result.topic.name)}>
        #{result.topic.name}
      </BorderLeft>
    {:else if result.type === "person"}
      <PersonInfo person={result.person} />
    {/if}
  {/each} -->
  <Feed {relays} {filter} />
</Content>

<Scan bind:this={scanner} onScan={tryParseEntity} />
