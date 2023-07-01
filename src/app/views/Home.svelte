<script lang="ts">
  import cx from "classnames"
  import {filter as _filter} from "ramda"
  import Anchor from "src/partials/Anchor.svelte"
  import Content from "src/partials/Content.svelte"
  import user from "src/agent/user"

  let topicBtns = [];
  let initTopicBtns = ["Dogs","Cats","Horses","Restaurants","Travel","Wine","Design","Spain","Search"];
  
  setInterval(()=>{
    let settings = user.getSettings();
    if(settings.topicBtns.length == 0){
      settings.topicBtns = initTopicBtns;
      user.setSettings(settings);
    }
    topicBtns = settings.topicBtns;
  }, 100);
  document.title = "Home"
</script>

<Content size="inherit">
  {#if !user.getProfile()}
    <Content size="lg" class="text-center">
      <p class="text-xl">Don't have an account?</p>
      <p>
        Click <Anchor href="/login">here</Anchor> to join the nostr network.
      </p>
    </Content>
  {/if}
  <Content size="inherit" style="height: 80vh">
    <div class="grid gap-y-[20px] grid-cols-3 text-center" style="margin:auto 0;">
      {#each topicBtns as topicBtn}
        <a class="btn-sqaure" href={`/search/${topicBtn}`}>{topicBtn}</a>
        <!-- <Button on:click={searchFilter(bookmark)} class="mt-4 mb-4">{bookmark}</Button> -->
      {/each}
    </div>
  </Content>
</Content>
