# Current

- [ ] Fix connection management stuff. Have GPT help
- [ ] Add zap splits https://github.com/nostr-protocol/nips/pull/552
- [ ] Integrate simplex sharing? https://simplex.chat/docs/guide/readme.html
- [ ] Add threads - replies by self get shown at the top of replies?
- [ ] Fix rich text -> plain text using library
- [ ] Highlights
  - Allow highlighting text in notes
  - When something is highlighted, show fixed-position elements for adding highlights
  - When a note is "selected" (what does that mean, on hover?), show annotations
- [ ] Relay reviews
  - New kind, d as url? Combine with labeling?
  - Show reviews in relay detail rather than events. Warn about events tab
  - Add curated relay list, and an easy way to view content on the relays
- [ ] Add preview proxy thing
- [ ] White-labeled
  - [ ] Add invite code registration for relay
    - [ ] Add endpoint to nostream that updates whitelist
    - [ ] Show invite code somewhere so users can share it.
    - [ ] Use a signed string so it's based on pubkey?
    - [ ] Nostream is public-read, apply registration to reads as well
  - [ ] Avoid leaking events from private relays
- [ ] Image classification
  - https://github.com/bhky/opennsfw2

# Core

- [ ] Make mutes private
- [ ] Support nip 94
- [ ] Show trending on search page http://localhost:5173/nevent1qqs8m3f52uelvpan4nn9hmc99a4qde0gjq9mlxc2khu057nryk3879gpr4mhxue69uhkummnw3ez6ur4vgh8wetvd3hhyer9wghxuet5qyv8wumn8ghj7un9d3shjtnndehhyapwwdhkx6tpdstjvu0y
- [ ] Add welcome.nostr.wine relay to onboarding
  - http://localhost:5173/nevent1qqsp9vf7agqyl7swhwepjw0r9s8ny55vsxkljh62pn0uh6f2g9z7a2qpr3mhxue69uhkummnw3ezuarjw43kketwvf6kx6mn9e3k7mgpyfmhxue69uhkummnw3ez6an9wf5kv6t9vsh8wetvd3hhyer9wghxuet5nds9yj
- [ ] Write multi-relay pagination into paravel and open source it
  - https://github.com/nostr-protocol/nips/pull/408
  - nevent1qqszpjf3307ccam3cl957yc7k3h5c7vpt7gz2vdzgwkqszsyvdj6e0cpzfmhxue69uhk7enxvd5xz6tw9ec82csgdxq30
- [ ] Nostr Wallet Connect https://nwc.getalby.com/
- [ ] Abort all context subs when navigating
- [ ] integrate media.nostr.band
- [ ] Add webtorrent support
  - https://coracle.social/nevent1qqsxgxcsq5vevy4wdty5z5v88nhwp2fc5qgl0ws5rmamn6z72hwv3qcpyfmhxue69uhkummnw3ez6an9wf5kv6t9vsh8wetvd3hhyer9wghxuet5qk6c9q
- [ ] Support key delegation
  - https://github.com/nbd-wtf/nostr-tools/blob/master/nip26.ts
- [ ] Attachments (a tag w/content type and url)
- [ ] Separate settings for read, write, and broadcast relays based on NIP 65
- [ ] Release to android
  - Steve says capactiorjs works well
  - https://svelte-native.technology/docs
  - https://ionic.io/blog/capacitor-everything-youve-ever-wanted-to-know
  - Or just wrap it in an apk
- [ ] Add no-relay gossip
  - Capture user events in a local db
  - Possibly release "local relay" as a library
  - File import/export from db, NFC transfer
  - Save user notes to db
  - Fixes when you hide something, but the event doesn't get retrived, and it gets un-hidden
- [ ] Compress events
  - https://github.com/nostr-protocol/nips/issues/265#issuecomment-1434250263
- [ ] Refine feeds
  - [ ] Trim feeds once the user scrolls way down to save on memory
  - [ ] Don't lose feeds when navigating, persist modals. Remember scroll state
- [ ] Add support for nests https://nostrnests.com/
  - nevent1qqs2jm46m9tg9z33av4fp8hhapsx96234htz75wrvpvrax5jjl8m5usprfmhxue69uhhyetvv9ujumn0wd68yurvv438xtnrdaksy4d92m
- [ ] NIP 39 Support https://github.com/nostr-protocol/nips/pull/201/files
- [ ] Integrate plephy https://plebhy.com/

# UI/Features

- [ ] Combine search and scan
  - [ ] Search for topics using nostr.band
  - [ ] Include notes in search results
- [ ] Use real links so cmd+click or right click work
- [ ] Allow sharing of lists/following other people's lists
- [ ] Add suggestion list for topics on compose
- [ ] Badges link to https://badges.page/p/97c70a44366a6535c145b333f973ea86dfdc2d7a99da618c40c64705ad98e322
- [ ] Add QR code that pre-fills follows and relays for a new user
- [ ] Advanced search: select timeframe, authors, p tags, t tags
  - If logged in, open a detail page that shows the relays and people
  - If not logged in, pre-populate follows/relays in onboarding flow
- [ ] If someone logs in with their private key, create a notification to install an extension
- [ ] Review QR codes, search, basic affordances for link navigation
- [ ] Add delete button to notes
- [ ] Log in as user button
- [ ] Separate notifications out by type, mute certain kinds. Likes are extraneous
- [ ] Relay recommendations based on follows/followers
- [ ] Make the note relays button modal make sense, one relay with no explanation is not good
- [ ] Linkify invoices
- [ ] Person zaps
- [ ] Apply person popover to mentions in notes as well
- [ ] Invite link, nprofile + path that prompts someone to sign in or create an account and auto-follow the inviter
- [ ] Polls
  - Find the best implementation https://github.com/nostr-protocol/nips/search?q=poll&type=issues
  - Comment on all three nip drafts which one I implemented
- [ ] Share button for notes, shows qr code and nevent
- [ ] open web+nostr links like snort
- [ ] Pinned posts ala snort
- [ ] Scroll to top button that appears after scrolling a bit
- [ ] Likes list on note detail. Maybe a sidebar or header for note detail page?
- [ ] Add keyword mutes
- [ ] Show options on note detail for retrieving replies
  - Replies from user's network
  - All replies from author's + user's read relays, including spam
- [ ] Improve publish notification with more information, retries, etc
- [ ] Use nip 56 for reporting
  - https://github.com/nostr-protocol/nips/pull/205#issuecomment-1419234230
- [ ] Sync mentions box and in-reply mentions

# Privacy

- [ ] Add image proxy to avoid leaking user ips to hosts

# Big Picture

- [ ] Monetize multiplexing
- [ ] Image Uploads
  - [ ] Add LN invoices
  - [ ] Contact other client authors, self-hostable, or just use my instance
  - https://github.com/ElementsProject/lightning-charge
  - https://github.com/nostr-protocol/nips/pull/250
- [ ] Add coracle relay
  - Authenticated write, public read
  - Only accepts events from people with a @coracle.social nip05
- [ ] Micro app DSL
- [ ] Groups - may need a new NIP, or maybe use topics or relays
- [ ] Tool for finding relays with light usage so people can spread out
  - Graph view? Query db with COUNT? Hardware specs on relay info endpoint?
  - "adoptarelay.com"
  - Add suggested relays based on follows or topics

# Chat/DMs

- [ ] Link/embed good chat/DM micro-apps
- [ ] Ability to leave/mute DM conversation
- [ ] Add notifications for chat messages
- [ ] Add "new DM" button to dms list


