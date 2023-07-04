<script lang="ts">
    import {onMount} from "svelte"
    import {nip19} from "nostr-tools"
    import {last, reject, pluck, propEq} from "ramda"
    import {fly} from "svelte/transition"
    import {writable} from "svelte/store"
    import {annotateMedia} from "src/util/misc"
    import Button from "src/partials/Button.svelte"
    import Compose from "src/partials/Compose.svelte"
    import Content from "src/partials/Content.svelte"
    import Heading from "src/partials/Heading.svelte"
    import user from "src/agent/user"
    import {modal} from "src/partials/state"

    let compose = null
  
    const onSubmit = async () => {
        let {content} = compose.parse();
        var settings = user.getSettings();
        settings.topicBtns = settings.topicBtns.filter(topicBtn => topicBtn != content);
        user.setSettings(settings);
        modal.clear();
    }
  </script>
  
  <form on:submit|preventDefault={onSubmit} in:fly={{y: 20}}>
    <Content size="lg">
      <Heading class="text-center">Delete a Button</Heading>
      <div class="flex w-full flex-col gap-4">
        <div class="flex flex-col gap-2">
          <strong>Write Button Name</strong>
          <div
            class="mt-4 rounded-xl border border-solid border-gray-6 p-3">
            <div>
              <Compose bind:this={compose} {onSubmit} />
            </div>
          </div>
        </div>
        <div class="flex gap-2">
          <Button type="submit" class="flex-grow text-center">Delete</Button>
        </div>
      </div>
    </Content>
  </form>
  