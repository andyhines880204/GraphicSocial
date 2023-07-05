<script lang="ts">
  import {filter as _filter} from "ramda"
  import Content from "src/partials/Content.svelte"
  import user from "src/agent/user"
  import { onMount } from "svelte"
  import {modal} from "src/partials/state"

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

  const {petnamePubkeys, relays} = user
  const needsRelays = () => $relays.length === 0
  const needsPeople = () => $petnamePubkeys.length === 0

  onMount(()=>{
    console.log("HOMEHOMEHJOMEHOE");
    console.log(user.getProfile());
    let islogin = needsRelays() ? "relays" : needsPeople() ? "people" : null
    if(islogin){
      modal.push({type: "smartlogin"})
    }
  })
  document.title = "Home"
</script>

<Content size="inherit">
  <Content size="inherit" style="height: 80vh">
    <div class="grid gap-y-[20px] grid-cols-3 text-center" style="margin:auto 0;">
      {#each topicBtns as topicBtn}
        <a class="btn-sqaure" href={`/search/${topicBtn}`}>{topicBtn}</a>
        <!-- <Button on:click={searchFilter(bookmark)} class="mt-4 mb-4">{bookmark}</Button> -->
      {/each}
    </div>
  </Content>
</Content>
