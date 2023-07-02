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
  <Content style="height: 80vh">
    <h1>About US</h1>
    <p></p>
  </Content>
</Content>
