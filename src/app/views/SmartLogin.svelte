<script lang="ts">
  import {uniq, reject} from "ramda"
  import {onMount} from "svelte"
  import {generatePrivateKey} from "nostr-tools"
  import {navigate} from "svelte-routing"
  import {fly} from "svelte/transition"
  import {shuffle} from "src/util/misc"
  import {displayPerson} from "src/util/nostr"
  import {getFollows, defaultFollows} from "src/agent/social"
  import {getPubkeyWriteRelays, sampleRelays} from "src/agent/relays"
  import {getPersonWithFallback, searchPeople} from "src/agent/db"
  import network from "src/agent/network"
  import user from "src/agent/user"
  import pool from "src/agent/pool"
  import keys from "src/agent/keys"
  import cmd from "src/agent/cmd"
  import {loadAppData} from "src/app/state"
  import {modal} from "src/partials/state"
  import Input from "src/partials/Input.svelte"
  import Anchor from "src/partials/Anchor.svelte"
  import Heading from "src/partials/Heading.svelte"
  import Content from "src/partials/Content.svelte"

  const privkey = generatePrivateKey()
  const profile = {}
  const {relays} = user

  if ($relays.length === 0) {
    user.updateRelays(() =>
      (pool.forceUrls.length > 0 ? pool.forceUrls : pool.defaultUrls).map(url => ({
        url,
        write: true,
      }))
    )
  }

  const signup = async () => {
    await keys.login("privkey", privkey)
    profile.about = "I am new user"
    profile.picture = ""

    user.updatePetnames(() =>
      defaultFollows.map(pubkey => {
        const [{url}] = sampleRelays(getPubkeyWriteRelays(pubkey))
        const name = displayPerson(getPersonWithFallback(pubkey))

        return ["p", pubkey, url, name]
      })
    )

    console.log("######################")
    console.log(user)

    // Re-save preferences now that we have a key
    await Promise.all([
      user.updateRelays(() => user.getRelays()),
      cmd.updateUser(profile).publish(user.getRelays()),
      user.updatePetnames(() =>
        user.getPetnamePubkeys().map(pubkey => {
          const [{url}] = sampleRelays(getPubkeyWriteRelays(pubkey))
          const name = displayPerson(getPersonWithFallback(pubkey))

          return ["p", pubkey, url, name]
        })
      ),
    ])

    loadAppData(user.getPubkey())

    modal.clear()
    navigate("/home")
  }

  // Prime our people cache for hardcoded follows and a sample of people they follow
  onMount(async () => {
    const relays = sampleRelays(user.getRelays())
    const follows = user.getPetnamePubkeys().concat(defaultFollows)

    await network.loadPeople(follows, {relays})

    const others = shuffle(uniq(follows.flatMap(getFollows))).slice(0, 256)

    await network.loadPeople(others, {relays})
  })
</script>

<div in:fly={{y: 20}}>
  <Content size="lg">
    <Heading class="text-center">Welcome to Graphic</Heading>
    <p class="text-center">Type your username and start using the app.</p>
    <div class="flex flex-col gap-2">
      <div class="flex flex-col gap-1">
        <strong>Your Name</strong>
        <Input type="text" name="name" wrapperClass="flex-grow" bind:value={profile.name}>
          <i slot="before" class="fa-solid fa-user-astronaut" />
        </Input>
      </div>
    </div>
    <Anchor type="button-accent" class="text-center" on:click={signup}>Start</Anchor>
  </Content>
</div>
