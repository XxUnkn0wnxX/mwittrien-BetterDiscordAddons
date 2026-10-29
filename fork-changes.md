# Fork changes

Last updated: **2026-10-29** (Australia/Melbourne).

This fork carries temporary BDFDB compatibility fixes for changes in Discord’s frontend classes. The aim is to preserve BDFDB’s existing class aliases and the plugins that consume them through targeted descriptors, mappings and owner-specific compatibility hooks.

Upstream remains [mwittrien/BetterDiscordAddons](https://github.com/mwittrien/BetterDiscordAddons). These changes are maintained on this fork’s `develop` branch until equivalent upstream fixes are available.

## Branches and scope

| Branch | Purpose | Pull / push convention |
| --- | --- | --- |
| `upstream-master` | Clean upstream comparison and recovery base | Pull from `upstream/master`; push to `origin/upstream-master` |
| `develop` | Fork-specific compatibility work | Pull and push through `origin/develop` |

The current comparison base is upstream commit `2cda4b90248e93558fe41311ba33af3cfc1261e8`. The October 28 repair checkpoint used `819bd99b00cc7870b7a82905ba0a950ca4ee228e`. The clean branch was renamed from `master` to `upstream-master` locally and on the fork, and is the fork’s default branch. The explicit name avoids confusing a local branch with the remote-tracking ref `upstream/master`.

The October 28 checkpoint completed the reported public-alias repairs on develop. Lookup results, native-export evidence and conditional UI validation remain separate below; the upstream comparison branch stays clean.

## Upstream sync — October 29

The fork includes all five upstream commits through `2cda4b9024`, including the PersonalPins 2.3.8, ImageUtilities 5.6.8 and BetterNsfwTag 1.3.4 updates. Both badge-related data changes now follow upstream: `Badge` discovery requires `numberBadge`, `base` and `iconBadge`, and the `badgetextbadge` alias is removed. The current data therefore exposes **1,619 aliases**. The other fork compatibility mappings, plugin code and stylesheet are retained.

The October 28 results below describe the earlier 1,620-alias checkpoint. They are not a new runtime acceptance result for this upstream sync.

## BDFDB resource downloads

[Library/0BDFDB.plugin.js](Library/0BDFDB.plugin.js) now obtains the resource metadata and JSON/CSS resources from this fork’s `develop` branch:

| Resource | Patched source |
| --- | --- |
| Resource hash metadata | `https://api.github.com/repos/XxUnkn0wnxX/mwittrien-BetterDiscordAddons/contents/Library/_res?ref=develop` |
| Data JSON | `https://raw.githubusercontent.com/XxUnkn0wnxX/mwittrien-BetterDiscordAddons/develop/Library/_res/0BDFDB.data.json` |
| Raw stylesheet | `https://raw.githubusercontent.com/XxUnkn0wnxX/mwittrien-BetterDiscordAddons/develop/Library/_res/0BDFDB.raw.css` |

The metadata endpoint deliberately has no slash before `?ref=develop`: the slash form redirected without preserving the branch query during verification.

The plugin’s upstream `@updateUrl` and `PluginUrlMap.BDFDB` entry remain unchanged. A future upstream plugin update can therefore replace this temporary downloader override.

The existing resource cache compares saved remote metadata with the fetched remote hashes. It does not hash the local edited resource file to decide whether it should be downloaded again. Local testing can therefore retain patched resources while the saved metadata still matches the remote branch. Missing resources, changed remote hashes, or a replacement plugin can cause resources to be downloaded again. Deleting the installed JSON or CSS during local testing can retrieve the remote copy before the local repairs have been published.

## Remaining 41 package — accepted lookup checkpoint

Prepared on **2026-10-28** against stable frontend **652418**, revision `5c8490b77d210a433203192c6e08fb7cd45550c8`, main asset `web.33a8a649a2fda558.js`. The final manual receipt at **2026-10-28 02:12:06 UTC** reached **0 unresolved aliases**, matching the **41 → 0** prediction. The installed package preserves all **1,620 public aliases**, the **1,579 previously resolved mappings**, existing native descriptors and lazy suffixes, and the **213 previously verified native provider fields**. The actual scan covered **314 groups / 1,620 aliases**, with no getter errors or malformed mappings. All **1,579 previously working class and selector outputs** matched the accepted baseline exactly.

The package adds an isolated `BDFDBCompat` class group. Verified native owners receive the necessary class markers through their existing props, render output or committed refs. The existing BDFDB plugin dispatcher and class/selector getters retain their behavior. Plugins continue to use their existing aliases.

| Area | Compatibility work |
| --- | --- |
| Editor and direct profile composer | Mark the actual editable leaf through `textAreaPaddingClassName`; preserve native input state, send behavior and current editor padding. The original direct composer uses `user_profile`; the separate contextual reply composer keeps its own behavior. |
| Server header and favorites | Mark the real dropdown SVG and its open state, including deferred popout render callbacks. Restore the favorites ring wrapper used by DisplayServersAsChannels. |
| Native dialogs and layers | Mark the actual attachment-edit and DM-add modal containers. Restore the precise hidden-layer state and identify backdrops through their committed native LayerContainer parent. |
| Profiles | Restore sourced banner/premium, avatar/bot/position, solo-emoji and explicit last-section predicates. The supported avatar hint remains a non-interactive overlay, preserving the current native action. |
| Other native UI | Restore the editable Card modifier, painted voice-quality paths, non-round small live tag, channel-topic body, text-preview download SVG, welcome glyph/edit button and private-channel avatar targets. |
| Styling utilities | Keep `textareaaccessorybar` and `goliveclickablegamewrapper` tied to their real current CSS tokens. Restore the proven legacy CSS utilities for `menuimagecontainer` and `unreadbaricon`. These four aliases make styling APIs available; they do not assert a current native node or invent an unread glyph. |

The compatibility code uses the React/JSX interception targets already used by BDFDB. Stable component identities survive library reevaluation; a mounted original functional component is deferred until a safe close/reopen rather than having its input state reset. Instance-field hooks have weak ownership and are cancelled on stop/unmount. Compatibility initialization failures preserve the existing plugin rendering callbacks. Generation guards prevent an obsolete initializer from publishing globals or writing downloaded resources after a newer initialization takes ownership.

Local checks cover all 20 current owner signatures across the captured asset catalogue, exact owner function hashes, the existing BDFDB dispatcher with BetterDiscord’s local patcher, deferred Header callbacks, memo/forwardRef legacy callback generations, input state/focus/refs, normal animated backdrop creation, cached-only helper discovery, failed initialization and stale request callbacks. **12,632 sibling getter-form comparisons** preserve the other 1,579 aliases. The temporary probe retains its 21 reviewed manual providers and distinguishes resolved tokens, CSS utilities and observed current UI nodes. Startup and opening settings perform no scan.

The final manual check also verified **21 native providers / 213 native fields**, the existing three owned styles and four utility fixtures. All **eight owners encountered during the normal UI check** prepared successfully, matched an audited source fingerprint and reported no failures. PersonalPins’ icons were confirmed visually by the user. The probe observed four conditional aliases in the current document; **33 other conditional aliases remained unobserved** after their UI was closed or because their conditions were not exercised. Their source and focused fixtures are recorded separately; this checkpoint does not claim every possible UI state was displayed.

Six owners have equivalent declarations in both the main bundle and lazy chunks under the same module IDs. Their exact source fingerprints differ because of minified identifiers. Both captured variants passed role, prop, import/call, predicate and JSX-shape comparison; the probe accepts only those recorded alternatives. Helper discovery identifies the original modal and premium namespaces, and revalidates finder results so a matched factory’s exports cannot be mistaken for its hook.

This work follows changes in Discord’s delivered frontend. Its applicability is tied to the frontend revision recorded here; an older BetterDiscord/Electron client can have additional independent compatibility limits. The fork preserves the existing plugin-facing API.

<details>
<summary>Remaining-package JavaScript assets and SHA-256 checksums</summary>

```text
Date: 2026-10-28
Base URL: https://discord.com/assets/
Build: stable 652418
Version: 5c8490b77d210a433203192c6e08fb7cd45550c8
Main: web.33a8a649a2fda558.js
Main sha256: 5b2afcf1dea50e7e6e98e460cf8dba00101ff37c4610363f7002719de4f7bb1c

03e702e18283989b.js [modules 993077]
  sha256 a9afb9650075944eaa555f5fc4bbb9e0e993f44c0cb75656f4331e44131bfe96
382b6917c686b013.js [modules 181094]
  sha256 994e142d3bcc4cc370befc4fbcd00f1a4511ca68ac6300dd1e6d6bb2ad7e88b4
4ea98eea20046cd6.js [modules 173842]
  sha256 af3c83009c1e71d0d2d7e8dd7686f298a3157667898c0524039bbc0cc3199db1
8a04b9d0f2487cb6.js [modules 999751, 452015]
  sha256 d0b8f8d3d836271a0d454a48cf8157df1182eec6e62156ffeae034a6aca2af95
8e1ca1a62f74fcbc.js [modules 718019]
  sha256 106c1a1dc950d6742a874eae52f323dd4f590ee5b6bc79f991819404e37035fd
9ffc96adc8627c5b.js [modules 900179, 915614]
  sha256 f65b0ca7edb56367155568dad2689e2aaf0b6aed4d7ea73b2ce93e0723fc2bb0
c230fc948a50fa03.js [modules 44788]
  sha256 5a967f53b5445c810167f3e6e2ccb7e791da1b21d67183001bae92cb3f97cb07
cf81c8968408d3de.js [modules 314307, 425812]
  sha256 d26bbb43bb9cddc16dc7faa1604bd31f2219b929c7eec8af0452106fc8217941
d29157b7d9faba67.js [modules 394816]
  sha256 d7dd8cc092deccaf5ac13e5fb1f66da4b727afbba672472350642ebb9191f5c5
d7e7be277ff35925.js [modules 402216]
  sha256 e69895a076b1eeb77e514e3c02108c2217cc30600ce3e1e46bdfaacc586afdda
d9ca6831baf622ca.js [modules 392553]
  sha256 c47f5345c654adeed7ddb60a39c3764297de4f6c88131203b27da37be6f3d7c7
e68d7398a600e53b.js [modules 799871]
  sha256 d04d9b0dee0b8e1c88458203e87248f4d64ed7e19667e68ee79d4b2b7f77b9b2
web.33a8a649a2fda558.js [modules 255434, 638495, 242816, 189213]
  sha256 5b2afcf1dea50e7e6e98e460cf8dba00101ff37c4610363f7002719de4f7bb1c
```

<details>
<summary>Audited main-declaration variants and copied native CSS</summary>

```text
channelEditor [module 392553, main offset 3382495]
  sha256 b5f2bd63fc6859556ae7b92df5a1ec1c584b8cf91b2bd360c5e2776cc734bd7b
plaintextTools [module 44788, main offset 6278927]
  sha256 876a1f3eb9e7f77c8ff8c934f3e7399b9ab2da8fe412eb08eacc34a7a0b6450f
profileBannerFill [module 915614, main offset 8362346]
  sha256 0045ac9b2600f53f128e70adeaa32e97b7c846b7d2babc515f2a8302fd327bdc
profileBannerType [module 915614, main offset 8363243]
  sha256 9ed61dd3307bf644176d3a075f1401d794b596416644e12fa0923f7ae4686eb7
profileAvatar [module 718019, main offset 8341680]
  sha256 6a06ea129254ce7dc2c5b04e631079c0f34ad237bc29cb0652b2a4c9d16abc93
profileSection [module 900179, main offset 8370047]
  sha256 d6199481c822267a8ea878cde94b1aec216d2a030f446707143a2764aa5fe925

962953.64344fbf982477be.css
  sha256 a29ae4869cf70278496c222bd163fee12139f894d149442d54ecce15f5a199b0
830171.7b749dea13d32e75.css
  sha256 1ab9fc86218524aac0003f008044e3bc3346f7f38eae35828b849f033edac173
```

</details>

Historical first-party client captures establish the old predicates and utility styles; they are separate from the current 652418 source above. The dated records include Canary build 275944 and the pinned [native capture](https://github.com/hackermondev/discrapper-canary/tree/5dbd49b2b7a1bb4ec2fce5813f2afbc7c17251aa).

</details>

<details>
<summary>Public aliases covered by the remaining package</summary>

```text
backdropwithlayer [owned-compatibility-marker]
cardeditable [owned-compatibility-marker]
textareaaccessorybar [native-css-utility]
textareafontsize12padding [owned-compatibility-marker]
textareafontsize14padding [owned-compatibility-marker]
textareafontsize15padding [owned-compatibility-marker]
textareafontsize16padding [owned-compatibility-marker]
textareafontsize18padding [owned-compatibility-marker]
textareafontsize20padding [owned-compatibility-marker]
textareafontsize24padding [owned-compatibility-marker]
userpopoutcustomstatussoloemoji [owned-compatibility-marker]
dmaddpopout [owned-compatibility-marker]
filedownloadbutton [owned-compatibility-marker]
goliveclickablegamewrapper [native-css-utility]
guildfavoritesring [owned-compatibility-marker]
guildheaderbutton [owned-compatibility-marker]
guildheaderbuttonopen [owned-compatibility-marker]
modalminitext [owned-compatibility-marker]
livetagsmall [owned-compatibility-marker]
menuimagecontainer [owned-css-utility]
messagemarkuptimestamptooltip [native-css-with-owner-hook]
messageswelcomelocked [owned-compatibility-marker]
messageswelcomebutton [owned-compatibility-marker]
messageswelcomebuttonicon [owned-compatibility-marker]
modallayerhidden [owned-compatibility-marker]
modallayerbackdropwithlayer [owned-compatibility-marker]
quickmessage [owned-compatibility-marker]
recentmentionsdmicon [owned-compatibility-marker]
unreadbaricon [owned-css-utility]
uploadmodal [owned-compatibility-marker]
userbannerpopout [owned-compatibility-marker]
userbannerpopoutpremium [owned-compatibility-marker]
userbannermodal [owned-compatibility-marker]
userbannermodalpremium [owned-compatibility-marker]
userpopoutavatarhint [owned-compatibility-marker]
userpopoutavatarhintinner [owned-compatibility-marker]
userpopoutavatarpositionnormal [owned-compatibility-marker]
userpopoutavatarwrappernonuserbot [owned-compatibility-marker]
userpopoutavatarwrappernormal [owned-compatibility-marker]
userpopoutsectionlast [owned-compatibility-marker]
voicedetailspingforeground [owned-compatibility-marker]
```

</details>

## Class compatibility changes

The production mapping changes are in [Library/_res/0BDFDB.data.json](Library/_res/0BDFDB.data.json). The October 28 checkpoint preserved all **1,620 existing public alias names**; the October 29 upstream sync removes one as described above. The earlier accepted checkpoints contained **126 changed data entries or fields** relative to their original clean upstream base. Subsequent accepted batches added one isolated native menu-image mapping, three owned style values, and five native activity/name-container repairs. The earlier checkpoint covered 69 distinct alias targets, followed by the 32 verification targets described below. Their shared fixes include native discovery and unloaded lookups as well as repairs to reported missing aliases.

| Area | Change and verified behavior |
| --- | --- |
| Badges | The October 28 repair matched `numberBadge` / `iconBadge` and restored seven base, icon, numeric and shape aliases. Its separate `badgetextbadge` repair is superseded by the October 29 upstream sync: discovery also requires `base`, and the text-badge alias is removed. |
| Image modal | Add verified lazy suffixes for `ModalCarouselNav` and `ImageModalMediaWrapper`, restoring the three navigation aliases and the media wrapper before their modules have loaded. |
| Animations | Add the current `Animations` suffix for the four mapped animation classes. |
| Screen sharing | Remove the obsolete `gradientTop` descriptor requirement and use the current `CallScreenshare` suffix for the three controls aliases. |
| BDFDB changelog | Point `_bdchangelogtitle` to the existing `BD.bdChangelogTitle` class. |
| Menu slider | Point `menuslider` to the current native `Slider.slider` root. The latest batch below also restores the separate menu slider-container alias. |
| Reply author | Point `messagereplyname` to the current reply bar’s `MessageReplyBar.name`, keeping the existing reply container/text/close aliases intact. |
| Image errors | Point `imageerror` to the actual `ImageWrapper.imageErrorWrapper` element. |
| Server folders | Use `GuildFolder.isHovering` for `guildfolderexpandedbackgroundhover`. |
| Title bar | Use `TitleBarNew.winButton` for the actual window buttons. |
| Modal stack | Use `ModalScrim.scrim` for the two backdrop aliases and `ModalStackLayer.layer` for the modal layer. |
| Quick switcher | Match the current `quickswitcher` / `miscContainer` owner and add its verified lazy suffix, restoring both aliases. |
| Favorites | Give `guildfavoritesicon` a separate one-property `GuildFavoritesIcon` owner. Reject the unrelated DM-row stylesheet that also exports a favorite icon. The legacy ring alias remains unresolved. |
| Arrow transition | Restore `directiontransition` from the real stylesheet class `transition__5fbe9`. The four direction aliases are also checked after resolving the transition utility. |
| Profile banner | Point `userbanner` to the actual profile-background fill class `banner__68edb`. Keep the four unsupported size/premium variants unresolved. |
| Profile status | Use the current status region, emoji and text classes: `container_ab8609`, `statusEmoji_ab8609` and `statusText_ab8609`. |
| Profile avatar | Use the actual avatar wrapper and clickable modifier: `avatar__75742` and `clickable__75742`. Keep the five unsupported hint, position and wrapper variants unresolved. |
| Selectable text | Point `colorselectable` to the current typography modifier `selectable__4bd52`. |
| DM activity | Point `dmchannelactivitytext` to `PrivateChannel.activityStatusText`. The separate legacy activity-wrapper and emoji aliases remain unresolved. |

### Thirty-alias batch accepted

This **30-alias batch passed the manual full scan**. The source contains **73 aggregate mapping entries or field changes** and covers **303 groups**, while preserving all **1,620 public aliases**. The batch predicted **95 → 65** unresolved aliases; the actual result was **65**, with all 30 targets matching and no newly missing aliases.

| Area | New aliases | Current target |
| --- | ---: | --- |
| Audit log | 4 | Native audit-log, ellipsis, timestamp and user hooks; add the verified lazy suffix. |
| Active call | 1 | Native call wrapper `wrapper_cb9592`. |
| Channel topic | 2 | Current topic and expandable modifier in a separate owner; the old modal content alias remains unresolved. |
| Recent Mentions header | 5 | Separate clickable name, heading, text span, guild breadcrumb and guild-icon roles. The old DM-icon alias remains unresolved. |
| Search autocomplete | 2 | Current results/date listbox containers. |
| Search icons | 5 | Native clear, icon, layout, container and visible modifier. |
| Search result groups | 3 | Native result group, channel-name text and blocked-result wrapper. |
| Mention message lists | 2 | Actual message-list container and date divider. |
| File attachments | 2 | The file card and filename/metadata column, rather than the outer positioning shell. |
| Server header | 1 | The actual clickable server dropdown control. |
| Neutral text colors | 3 | Current strong, default and muted text color utilities. |

The neutral-color changes preserve current semantic text roles. They do not claim identical historical pixels or direct rename ancestry. Their selected classes apply only color, without the separate note component's layout or sizing classes. Red and yellow remain unresolved where the proposed utilities would change the old theme-dependent color meaning.

The source comparison covers inspected build **650389**, running build **652094**, and published build **652418**. Exact class exports and applicable CSS are unchanged for the selected mappings. The reviewed renderer differences change a lazy dependency chunk ID, a require ID, or neighboring minified bindings; their relevant class applications retain the same roles.

Unloaded lazy lookups retain BDFDB's existing primary-token behavior. A composed native export such as `clear__0c4c4 icon__0c4c4` returns the primary token before its owner is available; loaded results retain the full native composition. This batch does not rewrite that mechanism. The existing `Text` lazy suffix is unchanged.


### Thirty-two-target batch accepted

Source and local validation date: **2026-10-28**, Australia/Melbourne. The installed candidate preserves all **1,620 public alias names** across **312 diagnostic groups**. It adds **53 data-field changes** to the previous checkpoint, bringing the fork comparison to **126 changed data entries**. These changes passed source review, local production-getter checks and the manual runtime gate at **2026-10-27T21:08:34.369Z**. All 32 target class strings matched, and the separate status-editor finder returned the complete expected export.

The 32 verification targets comprise **15 repairs to reported fallback aliases**, **13 corrections to unloaded popout lookups and their cache isolation**, **two stale native-selector repairs**, **one native API alignment**, and **one module-finder repair benefiting eight existing aliases**. This is 32 verified targets across shared repairs, not 32 independent defects. The pre-test prediction was **65 missing → 15 intended fallback repairs → approximately 50 remaining**. The actual post-test total was **50**, with no newly missing aliases, getter errors or undefined results.

| Area | Targeted correction |
| --- | --- |
| Combobox | Use the current combobox, item and item-label export for three aliases. |
| Live indicators | Separate the current live wrapper, gray variant and large badge owner. The round and non-round branches differ; the old small-state alias remains unresolved. |
| Text badge | Point the removed legacy text-badge lookup to the current native badge root without changing the other badge aliases. |
| Profiles | Restore the role-list container, mutual-guild icon/no-icon alternatives and the section wrapper used by the actual compact profile popout. |
| Go Live, servers and members | Identify the current Go Live actions, server-discovery container and member activity wrapper. Server discovery excludes the separate Apps and Quests routes. |
| Menu slider | Match the current one-field menu-control wrapper, distinct from the slider itself. |
| Existing native hooks | Restore the reaction display-name wrapper and keybind value wrapper, and align the PersonalPins channel-name hook with its current native API. The old channel label already had BDFDB-owned CSS. |
| Popout fallback hashes | Move 13 supported aliases to a current internal group with the verified `_432f4` suffix. Their loaded native owner is unchanged. This fixes unloaded lookups and isolates them from the legacy scrolling-footer lookup that overwrites the old group cache. |
| Custom-status editor | Replace the retired `header` fingerprint requirement with four current properties and the actual 15-field export shape. All eight existing aliases and their class strings stay intact. |

The popout's old `scrollingFooterWrap` alias, group and suffix remain unchanged and unresolved. Its retained stylesheet rule does not establish a current native export. The current popout stylesheet is lazy-loaded. The accepted receipt lists `eaf6028a1440e4c0.css` as linked; this establishes stylesheet availability in that run, without independently proving PersonalPins' visual rendering. No stylesheet is force-loaded by this patch.

The status-editor finder needs separate runtime evidence because its eight strings were already correct through lazy fallback. The temporary probe records the original cached group and, only during the manual full scan, makes a separate paced call to BDFDB's public finder. The accepted fresh result matched the exact deployed descriptor and all 15 expected fields and values. The original group snapshot still contained a synthesized class entry, so this proves the repaired finder rather than a change to the existing class-group cache. The manual sequence is: reload Discord, open and close the custom-status editor, then press the full-scan button. Startup and opening the probe settings perform no module scan.

Local checks cover all 32 loaded/unloaded getter targets, **113 known native-token outputs**, and **1,588 unchanged sibling aliases**. Additional checks reproduce the old popout cache failure, exercise the legacy orphan before/between/after reads with loaded and unloaded owners, and prove that the old status fingerprint fails while the new fingerprint finds the actual export. The final production-getter run took approximately **2.16 seconds**. These are local fixture/source results, not a live Discord receipt.

This batch was inspected directly against stable frontend **652418**, version hash `5c8490b77d210a433203192c6e08fb7cd45550c8`, main asset `web.33a8a649a2fda558.js`. A public bootstrap check at **2026-10-27T20:57:38.458Z** still served that revision. It is a frontend/module identity, not a backend deployment identifier.

<details>
<summary>Current batch JavaScript assets, module IDs and SHA-256 checksums</summary>

```text
Base URL: https://discord.com/assets/
Build: stable 652418
Version: 5c8490b77d210a433203192c6e08fb7cd45550c8
Sentry: 2026-10-27-5c8490b77d210a433203192c6e08fb7cd45550c8-discord_web
Built at: 1793113957288 (milliseconds since Unix epoch)

09ba38cf8a5112d2.js [modules 577593]
  sha256 c0686dd29ebe67b677148dfbfa9aaba3f236cf893815f60d27c084324f0fb366
10bc7c0638948f18.js [modules 247261]
  sha256 f4fa3b846894f5065210224d0aafd82fa35357cd27e211f74171e02073bc42c0
110425.46a603d20543adb5.js [modules 874888]
  sha256 25ecfe66b3230c2f033e2497bbf0a499f4862741d99cd7bbdc6b70f75cc231bb
125eed40b24e0032.js [modules 635242]
  sha256 f00a4527506863f864ed3388eda06715bee13cb74932a49d2aedcde4be678e02
1bce0236639292ba.js [modules 945830]
  sha256 f88db7e6b61f73bdb52d5541076f8295c89ceb623c634727d98892283f2e7898
1d9944b5a565c718.js [modules 68450]
  sha256 1a4779451c73a174704a7d4796abd6498cb81b586afa23e783b1f2b0924b6e36
3485a255d0c8c218.js [modules 166005]
  sha256 36a825865703d1ffaf44374e79191b9ae484bbb6abdb2385650fced192155f0c
384209.5f531bc85f869b5d.js [modules 963434]
  sha256 53361e8ceba3ecccd140f56025176269fd9c4898b47d47b841d96bf386f08c5d
43eb9ce26873201a.js [modules 657977]
  sha256 b59782a7b637075b4630e1a8da8efd7ba3241f636b8fc77bbdfcc85f55a080ea
4ea98eea20046cd6.js [modules 173842]
  sha256 af3c83009c1e71d0d2d7e8dd7686f298a3157667898c0524039bbc0cc3199db1
535308.64580ddd66d53c21.js [modules 161185]
  sha256 2acf7082b05ad3d5713ec9dab083b15435e6bb704f5ae5b05eb10d279422cfc0
56606.9b22985f3cf034eb.js [modules 996205]
  sha256 a1192e130faee5091802a166196e92264ba25a710b11090818d88d97adb422d1
5c0f9ba48d69b587.js [modules 942262]
  sha256 5fffbe437121e5c730817ce3c57489e7eeae024d6b8c9d223e56f0099dbb5734
66607a54f94cc009.js [modules 714539]
  sha256 8e5ab88ee701a717910a1faa8aad2d5cf207b9d34429c88728c3b9a021acba67
67e30604a92e0b3f.js [modules 877149]
  sha256 4d0b49dc8d3da2ced8c0b3d7efa452ea1df5c370b06409c7371267438e37f064
6e5b8da464b02ede.js [modules 317070, 409757]
  sha256 e50ca7f1d9a71f2d07095364c04b082bd949a213f00ee856c2f90bcb0af0ec37
7b1a4db3e67a2fe2.js [modules 112753]
  sha256 c0b2ea1e0b59a60e5a88c294031e203077e49d90c5c5cb60fb8495eca1de9305
82cb2811a0004053.css [modules ]
  sha256 3338e1f5a65790a413837090f3bfedef439e06aa618965a6ea3500567f227991
830171.0a7ee0ba708d0c2f.js [modules 79958]
  sha256 6a78e21ab603a333b84d67403f9d18093f542ebba76829572b52bc56e3930561
8a04b9d0f2487cb6.js [modules 298242, 486487]
  sha256 d0b8f8d3d836271a0d454a48cf8157df1182eec6e62156ffeae034a6aca2af95
8ef2441f0248738d.js [modules 272917]
  sha256 2ca7812bb99427f5e439cb77b8c47ff96b65afd393a3a205eb53345eb2152081
8f6aa2de0aeace08.js [modules 190199]
  sha256 ff73ea48fd4f7a30f6e93efddaffb96db501af9b51bff4a18590157ce7728d16
971082.64a31dd2b20f301b.js [modules 889553]
  sha256 46793ec0b4964ff27182f17152ad49bbc8c8fbb050c979c604f74c80359eea0c
986e56bc29420c57.js [modules 51943]
  sha256 82e02ec5bccd08312143f1ab6cfc32d1add1f11fbc96d039640f04b12e6aa89d
a9b65c4f0f4fcaba.js [modules 434367]
  sha256 670ef507194d52f0b717e305afdfbcce236a651ea245eff5723aa5b47bbc0b7f
b240d733260cf9cf.js [modules 545760]
  sha256 2b406075759041d00dd94d617262904606634c98269ea5cc70c4f21e280a1f7f
fa07ff6bfd4a69d3.js [modules 9570]
  sha256 4b068d2832ed5f206b6be709a157088788c9e3a3659bf741d8adbbb6f568edf4
fd7ec6e2a703b0d8.js [modules 503026]
  sha256 e9219c5dad809b92e0ed07789c9b593a189a294ac522929c4f86d703e90e87a0
web.33a8a649a2fda558.js [modules 62010, 115486, 193663, 286134, 402216, 442228, 589158, 856488, 900179, 964355, 964570]
  sha256 5b2afcf1dea50e7e6e98e460cf8dba00101ff37c4610363f7002719de4f7bb1c
```

</details>

### Ownership and state boundaries

Several checks prevent a returned class string from being mistaken for a correct repair:

- The old `ModalLayer` fingerprint accepted a Checkpoint/year-in-review stylesheet with `layer` and `hidden` properties. Its `hidden__6b0e6` class belongs to that feature. The descriptor now identifies the current global `layer` / `inactive` owner and rejects the stale cached Checkpoint match. `modallayerhidden` remains unresolved.
- The profile banner mapping identifies the inner element carrying `backgroundImage` / `backgroundColor`, rather than the separate outer frame. It uses BDFDB’s existing lazy mechanism without an ambiguous `banner`-only discovery predicate. Its synthesized class map is not reported as discovery of a Webpack export.
- The status root is the current **status region**. The same class also wraps the self-profile **Add Status** prompt, so matching it does not prove that a nonempty status is present. The hidden reference/measurement container is a separate class.
- The current status emoji class covers both solo and text-plus-emoji forms. CSS sibling selectors distinguish those states. The legacy solo-emoji alias is not mapped to the general emoji class.
- The shorter `customstatusemoji` / `customstatusicon` aliases have a different legacy owner contract from the displayed profile status. They remain unresolved while the editor’s exact replacement is investigated.
- The arrow transition token is present in Discord’s stylesheet but absent from its native JavaScript export. Diagnostics identify this as a stylesheet-backed utility. It is not evidence that the native arrow renderer applies that utility automatically.

No compatibility stylesheet rules have been added at this checkpoint; [Library/_res/0BDFDB.raw.css](Library/_res/0BDFDB.raw.css) is unchanged. No BetterDiscord core or Discord desktop code is changed by this patch. The inspected desktop client receives the current Discord frontend, so an older desktop shell does not imply frozen frontend class names.

### Native menu image and three compatibility styles — accepted

The final audit checked all 50 remaining aliases against the captured class-export and stylesheet indexes for stable build **652418**. The menu renderer supplies one verified native replacement: `menuimage` now targets its `imageAccessory_c1e9c4` image element. A separate internal owner with the verified lazy suffix keeps the retired `menuimagecontainer` alias unresolved. The existing native menu mappings remain intact. Three further aliases have small BDFDB-owned styling replacements; those three are not native Discord element selectors.

| Alias | Mapping and behavior |
| --- | --- |
| `menuimage` | Uses the native menu image through `MenuImageCurrent.imageAccessory`; supports loaded and unloaded lookups without fabricating a legacy image-container hook. |
| `colorred` | Retains `TextColor.colorStatusRed`; an owned class applies `--status-danger`. |
| `coloryellow` | Retains `TextColor.colorStatusYellow`; an owned class applies `--status-warning`. |
| `tooltipyellow` | Uses `BDFDB.compatTooltipYellow` for a warning surface, text and pointer. The other 14 native tooltip aliases keep their definitions and discovery. |

The four appended rules in [the raw stylesheet](Library/_res/0BDFDB.raw.css) leave the previous stylesheet intact. The arrow inherits `--tooltip-pointer-bg`, so callers can still supply an explicit background, font color or pointer override, including when they explicitly pass the yellow class. No library JavaScript change is needed for these styles.

Local checks pass for all 1,620 public names, all six getter forms for the three targets, 19,404 unchanged sibling outputs across the two fixture states, and the current 16-field native tooltip owner with its 14 existing aliases. The finder fixture rejects unrelated owners. These checks model the installed host's class normalization; they do not establish browser CSS results.

The manual full scan at **2026-10-27 22:02:51 UTC** passed all four repairs: **50 → 46 unresolved aliases**, with no newly missing aliases, getter errors, undefined results or malformed mappings. It covered **313 groups and all 1,620 aliases** on stable frontend **652418**. The three applied-CSS checks passed in the running browser, including explicit background, font and pointer overrides; the hidden samples were removed. These checks do not invoke the tooltip constructor or certify every UI consumer. The overall class goal remains open for the other 46 aliases.

<details>
<summary>Source revision and stylesheet evidence for the three styles</summary>

```text
Source audit date: 2026-10-28 (Australia/Melbourne)
Frontend: stable build 652418
Version: 5c8490b77d210a433203192c6e08fb7cd45550c8
Main: web.33a8a649a2fda558.js
Palette CSS: 952007.87dea5a700fabeaf.css
  sha256 9a2f27f806880de7bdf8310dd122b67dfba5017b5a61da41b1830f0434667b33
Native menu JS: 261000.1b6532d3649c0f52.js [style factory 798819]
  sha256 25ed48f88a65c8ba77ad2c83500aaed195ed79cf7e691357a200b2a9aa9167a1
Native menu CSS: 261000.04eb3f5d741e2f7c.css
  sha256 11362b7d66fa8d80f494bf0afea6ce60713fd676aabd37f701a5f67927fffc5f
Native tooltip CSS: 348181.99d322304a4170d4.css
  sha256 a42d2cbd97134ccf613b5e6812baceadb169fbb7bb77c177ee6735da351e7564
The palette, menu and tooltip CSS assets are listed by the accepted live build receipt.
```

</details>

### Activity icons and name-container root — accepted

The **2026-10-28** source review adds five native repairs for stable frontend **652418**, revision `5c8490b77d210a433203192c6e08fb7cd45550c8`, using `web.33a8a649a2fda558.js`:

| Public alias | Current mapping | Native class |
| --- | --- | --- |
| `namecontainer` | `MemberActivityCurrent.container` | `container__91a9d` |
| `dmchannelactivity` | `CustomStatusIcon.container` | `container_c9d15c` |
| `dmchannelactivityemoji`, `customstatusemoji` | `CustomStatusIcon.emoji` | `emoji_c9d15c` |
| `customstatusicon` | `CustomStatusIcon.icon` | `icon_c9d15c` |

The original `CustomStatusIcon` group and its two public mappings are retained. Its fingerprint replaces the removed `textRuler` property with `customStatusContent`, and its verified lazy suffix is `c9d15c`. The upstream author's [2021 preview](https://github.com/mwittrien/BetterDiscordAddons/blob/70767d645867078352d06d579c7dd8358df9e568/Plugins/_res/DiscordPreview.html) places `customstatusicon` on an activity icon; the current activity and emoji renderers assign the corresponding fields from owner `607013`. These shared activity classes also occur outside DMs, so consumers requiring a DM-only selector still need the appropriate ancestor scope.

The name-container repair uses the exact `container__91a9d` value preserved in [upstream history](https://github.com/mwittrien/BetterDiscordAddons/blob/4ca5c82a26ca7293f1a247854d326bcd3e5811b4/Library/_res/0BDFDB.data.json) and the current member-row renderer. The existing six `NameContainer` sibling mappings remain intact. The shared AvatarWithText `layout__20a53` root is a different node and is not used as a substitute.

Focused checks against the production finder and all six class/selector getters pass for the five targets in loaded and unloaded fixtures. All 1,620 alias names remain, and 29,070 sibling getter comparisons are unchanged. This batch changes five data paths and adds no production JavaScript or CSS. The manual full scan at **2026-10-27 23:24:06 UTC** passed all five repairs: **46 → 41 unresolved aliases**, exactly as predicted, with no newly missing aliases or getter errors.

The local verification probe now uses `BdApi.Utils.loadEntry` for a closed list of 21 source-reviewed class providers during the manual full-scan action. It checks fresh BDFDB discovery, complete expected class fields and native export identity separately from alias results. The manual run verified all **21 providers and 213 expected native class fields**: two providers were already available, and 19 were initialized through the loader. Build checks, preload-wrapper handling, cancellation and non-overlapping audit paths pass focused tests. An unavailable loader or an unrecognized bootstrap factory is reported explicitly. This diagnostic loading does not count as additional permanent mapping repairs, and a warmed scan does not establish cold-start or native stylesheet availability. Startup and settings opening do not run the audit.

<details>
<summary>Build-specific source assets for these five repairs</summary>

```text
Source review date: 2026-10-28 (Australia/Melbourne)
Frontend: stable build 652418
Version: 5c8490b77d210a433203192c6e08fb7cd45550c8

Main: web.33a8a649a2fda558.js
SHA-256: 5b2afcf1dea50e7e6e98e460cf8dba00101ff37c4610363f7002719de4f7bb1c

Member class owner 996205: 56606.9b22985f3cf034eb.js
SHA-256: a1192e130faee5091802a166196e92264ba25a710b11090818d88d97adb422d1

Activity class owner 607013: 691398.d73af4576252e14c.js
SHA-256: 33a6fa2aa3588d1c453dd4d9d888ad66271f1b377f5b03118a802226171dd5a2

Activity icon renderers 835072/864436: f0ff497306e10c95.js
SHA-256: 2757cad778eccba8c5a45fb012299e9e6058096b99cf2aee0d8b5b482758dbe6

Emoji renderer 51183: aab264f3be51e622.js
SHA-256: 4f7afc544b6b2826dedfdda8b29aa6100427a9217c241e4c26a0f193e6f643bf
```

</details>

## Verification checkpoint

### Module revision targeted by the source audit

The inspected [Discord app bootstrap](https://discord.com/app) exposes the following frontend build metadata. This identifies the web/module revision used for source inspection; it is not an API-server deployment identifier.

| Field | Inspected revision |
| --- | --- |
| Release channel | `stable` |
| Discord build number | `650389` |
| `VERSION_HASH` / Sentry build ID | `331394cb000b8d4a09e9f33ac8af8c4de0164509` |
| Sentry release | `2026-10-26-331394cb000b8d4a09e9f33ac8af8c4de0164509-discord_web` |
| Build timestamp, UTC | `2026-10-26T07:20:58.030Z` |
| Main JavaScript asset | `web.ad0b95d8568e0f6b.js` |
| Source-audit and fix date | **2026-10-28**, Australia/Melbourne |

The fixes are tied to this inspected module snapshot and the exact asset checksums below. Shared asset filenames can remain unchanged across several Discord builds; a newer build number alone does not establish whether a particular class module changed.

A fresh anonymous bootstrap check at **2026-10-27T17:29:46.267Z**, reconfirmed before deployment at **2026-10-27T18:30:44.331Z**, serves build **652418**, version hash `5c8490b77d210a433203192c6e08fb7cd45550c8`, with main asset `web.33a8a649a2fda558.js` and release `2026-10-27-5c8490b77d210a433203192c6e08fb7cd45550c8-discord_web`. This newer published revision is recorded separately and has not been substituted for the source-audit target.

Before another consolidated deployment, the running renderer revision and the expected reload revision must be identified. Candidate exports, their rendering roles and the relevant CSS rules are compared with those revisions before the live files are replaced. A changed overall build hash requires review of changed owners; unchanged relevant modules can establish applicability across builds. Candidates whose applicability remains unknown stay pending.

A separate, user-collected DevTools identity receipt confirms the running frontend as stable build **652094**, version hash `3b37f498429022557efaa0dc32bc69397ab8b8a2`, main asset `web.31601baf446a9c02.js`, and release `2026-10-27-3b37f498429022557efaa0dc32bc69397ab8b8a2-discord_web` (built **2026-10-27T07:21:28.463Z**). Its captured main-asset SHA-256 is `df11933bcad0bf4260a2e41409b897d393111f4deed8a7f4d61152e8e5a43104`. This establishes the running build at metadata collection; it does not retroactively identify the build used by earlier reports.

The accepted manual reports below verified class results on the installed client, but did not yet capture its `GLOBAL_ENV` build metadata. The next consolidated manual report will record the running frontend build and main asset name. A cached asset filename or cache modification time alone is not treated as proof of the active renderer revision.

<details>
<summary>Inspected asset filenames, module IDs and SHA-256 checksums</summary>

```text
Base URL: https://discord.com/assets/
SHA-256 values below were calculated from the captured file bytes.

JS 18146.6a018778250053bd.js  [modules 195572]
  sha256 205d8519c69a9c8fd0c665dfb8223d3042721e8f1487cb03a344bcb90e23fb86
JS 196070.1af7d11b02adaf9d.js  [modules 22405]
  sha256 bcbfea4fa7186a98cda7dad949550c751ee947d2abf08a67333926c4b17923d9
JS 487458.832171e7c73e5bd7.js  [modules 827045, 676737]
  sha256 6401d01e372027c10dfbe693fe12901d8911cdfdae0c41ca1977b356d16b2281
JS 5257.909ec98dc9c8c26a.js  [modules 306091]
  sha256 106f24f332cdf27af59c559fdf6c2e33040d01fdd668080d307bb64fc9b40129
JS 55980d0fd23e2a95.js  [modules 418450]
  sha256 6a569ddecb7c608853a44891ad5e6f6c6dbc1396e6be071e7ca2dd27b9dbbb06
CSS 570385.76e58bb413ace330.css
  sha256 47ca638e7b566b591bf7868762f9c2faa19d05bba25506d37a1a7a8e8f38312c
JS 570385.7ebd93a9e755ab9a.js  [modules 103335]
  sha256 c04c8868b04d562d5bbe592fecd8231311dbc0251601f2e835a9a3da116ad039
CSS 58495.ad5de67af9a66ad3.css
  sha256 4b29995fd0ad922b884d7f220b17053b5cf89e7b47f91404630f58a3bd429d5e
JS 58495.ec25cbd8979acd27.js  [modules 565890]
  sha256 a95276188706b4bff54d91da13aeb71dbfc76c6cdfddc7d1692824273195ae85
JS 599266.7354acb10f72564f.js  [modules 678436]
  sha256 6e27a2a13f3c9f29779ae775601e567f2abea101b274a777c87bbd43434c7770
JS 686889.b3eee124af2579fd.js  [modules 774702]
  sha256 fe56bb3462cc9f681be356fc3039a29d70abd5ef3b77054964e94b7682b4436b
JS 773047.0e0c5f1b8bbd1893.js  [modules 108306]
  sha256 bb3f838a6997eb18ec8e1c68589ad3e959942f9bc38879ae5c9bf3771b9a386e
JS 818860.8a09c4fdb32131ac.js  [modules 327580]
  sha256 59c52af0ccf5637ada4166b16a8510ceb56dc85611dac1ceaf76a0cd1b5908d0
CSS 82171.3438527a5e2816c3.css
  sha256 8cb5fdea305e7cbf16afbf5fc880145136dc261186302d537ecd18dd6bcf5c4c
JS 82171.9ae1d50deeab1584.js  [modules 502622]
  sha256 c393adb4506569aac0d6eecf62e40b5c785f5d534695f5884d41885bc2da1de8
JS 834050.cae66cf5b187cd46.js  [modules 763097]
  sha256 7defe60b7f38ddc5af03e4c5dc605caac06606b2b8afa9d490eee66b42ee8d3c
JS 875762.b9e57c5787edeebc.js  [modules 329801]
  sha256 2dc9aed8cad79c8f46309a4fa4bc4517d586ffbe7ac9ba2867ed26e24ad33ab9
JS 89100.73a7136a1f12c69e.js  [modules 558497]
  sha256 7314313ba64249243cada1e56738412a62d4f220567ad05a92b84de1226e5a13
JS 898957.344e52b3bad9f32e.js  [modules 967481]
  sha256 d6345bc0cf68f9798e3d6769916bb7503dfbeef84d8dc494caae4b85ddb79aa7
JS 964940.44d8b7d96b89f8ec.js  [modules 943320, 409325]
  sha256 21dae1ddc19bd0394b20a14d41ad49de3bc5a60cc5bab5093ccfdaf9fe46ae21
JS 985768.619db79f2454ffae.js  [modules 303707]
  sha256 fafcaf2a41856e2f4e9d49418d0bd8c8cf7ea6759a8b6e10b80b231bb62afbdc
JS web.ad0b95d8568e0f6b.js
  sha256 3f215e2a4d156120449c090a08973d2e7d96da0d9736a4bb79928829a0dfd4e0
```

</details>

<details>
<summary>Thirty-alias batch source assets and compared renderer checksums</summary>

```text
Base URL: https://discord.com/assets/
Inspected build650389; exact source bytes compared with652094 and652418.

JS 215e7906dc720713.js [modules 979882]
  sha256 34ea8393fe8fa9fb61bfafd95f89c557451387acdcd7bcd79b1533e5d13c2153
JS 355502.24aa14d8554f2e19.js [modules 607226]
  sha256 483994895ab1961a9fd4f59e7c1daa3b512881332d46a1dcba1ee8e5dac0f6f6
CSS 355502.688919247693a682.css
  sha256 3473352b5d8720ba35ba2d957ae5780473eceb733b1b9b4a13d0dd4fbad6e3b4
JS 3cb2d1174f129135.js [modules 884496]
  sha256 ea8201a11ff2bec980be1a3781034d3e152a6e111b6d72bc3bbd6908fcb1675d
JS 4a1d6fc7dd0222f7.js [modules 948914]
  sha256 ab85d8c87d824bf7cb5808f7ac500b8e1dc29d22dd8bb6cf3d4c9fc748999a3e
JS 56483c8cf0f998b8.js [modules 420344]
  sha256 89845e078d895e9904af576f126522fc21072da375d72c3f3c677bd91d88dd6e
JS 598476.e73b985efae0acdd.js [modules 267283]
  sha256 8f33a3b07783bd31d4eb2c105fc60ca099731ef5dddda730e7dba18d6ff9133d
JS 6a08bc54740391c8.js [modules 540462]
  sha256 15669d57ae0213895f6673197a7dc62b65f4f9abbe544e0c2bb7065003aeb9f7
JS 6ede1713036b99d8.js
  sha256 bc97fa865647e0ad8ef22b03fbba7d2e4770c866c1b891c6d7266173e48b352b
CSS 73946.3a7e218fd8460ae6.css
  sha256 46356200dd22536624b8af5d0a3c9cfddd9376e9d16a40c37ae72cec733026dc
JS 73946.b2867044174f2552.js [modules 164503]
  sha256 8be8ccdcf00cf6c4c519554e90437afe952a1ca56b466333d7ea34e80a753117
JS 773047.0e0c5f1b8bbd1893.js [modules 108306]
  sha256 bb3f838a6997eb18ec8e1c68589ad3e959942f9bc38879ae5c9bf3771b9a386e
CSS 773047.76edaf18f2188bba.css
  sha256 a6fc96953de7579bdd104a8c7d3b76fd3efa0e9888668501c7fc9b3c7c975fa5
JS 77413.38937d20c29ce8c8.js [modules 745268]
  sha256 00ba0524176ba0864e30e480e2811d9ca4b3a9b72817861b17da2fc9983e66bf
JS 784573.e37b65dc8fda733f.js [modules 880162]
  sha256 46064ae73d984f02bbb15630519a0b013822a0539fccaac4a5fcbbf60f06abbe
JS 88e13aa2d572cfb1.js [modules 743373]
  sha256 573a8cd28baab05725be1aa7e66fd0e1f995898e40ebf735eeaabebbb9c5a284
JS 8eaa2a3f558031ab.js
  sha256 602b733ed3131982a80ac390f33161d3c3f63c05f5dac63eb891d790852265d6
JS 92a208c82c792a3e.js
  sha256 d17c9ab9e390ccba69569a1c55b9f66f5540b2ed197f82b162ec40f25fb05245
JS b240d733260cf9cf.js [modules 545760]
  sha256 2b406075759041d00dd94d617262904606634c98269ea5cc70c4f21e280a1f7f
JS bc6d2a92e1a16466.js [modules 165259]
  sha256 06436e805315dce492f3bdd33b344dda95e99d7b7b7ec74c7fac79e0b572a221
JS c5b5c25ec8ff16aa.js
  sha256 a134578d6a17cc6c2b7c79868de87fbc43bd20c7e410ef6bf05667ed97d5103e
JS e8ddf0dcb62e34f7.js [modules 565265]
  sha256 90d4c54fcb000fdd63f842c0a633511a338f98c60c00ddc9426112081aa6ff08
JS eb8033f20a624ce8.js
  sha256 df5224e2886aa6a906686bbfc6963d4ef986c9081ffa4550a0acbb5de2ed840e
JS ed98a484a1130077.js [modules 486506]
  sha256 e37e0ab216a4a43ca75fa61df456635e894fbefd3f3dc7d7f5a96aac4c592186
JS fa7ec5886f4d654f.js [modules 285512]
  sha256 38cb1733838e7dc58467622ecf08bc73d82563ed3b65c22ed57fe5507fdfaea9

Changed renderer assets used by compared builds:
JS 4ea98eea20046cd6.js [builds 652418]
  sha256 af3c83009c1e71d0d2d7e8dd7686f298a3157667898c0524039bbc0cc3199db1
JS 713e20c012ef7b05.js [builds 652418]
  sha256 abca3fded049674ac7f56936d3c9cb5464c743ba883205d6a79b1da3c257b278
JS 824205f94cb13afc.js [builds 652094]
  sha256 f9042b9fcb312c555dc598e8f754bb1130019abc6e03ca2fe121f60894869b4c
JS f1b21daced4ed09a.js [builds 652094, 652418]
  sha256 493d5d925cafc9a98ae668e5078b2d4bf6c9584e14f25b53007936698d237d29
```

</details>

### Live lookup results

The latest accepted manual full scan ran at **2026-10-28 02:12:06 UTC**, on stable frontend build **652418**, revision `5c8490b77d210a433203192c6e08fb7cd45550c8`:

| Check | Result |
| --- | ---: |
| Groups covered | 314 |
| Unique aliases covered | 1,620 |
| Aliases returning non-fallback strings | 1,620 |
| Missing-class fallbacks | 0 |
| Getter errors / undefined results / malformed mappings | 0 |
| Previous class and selector outputs preserved | 1,579 |
| Native providers / verified fields | 21 / 213 |
| Observed owners with prepared handlers and accepted source hashes | 8 |
| Controller failures / source mismatches / matched unprepared owners | 0 |
| Scan duration, including provider loading | 18.58 seconds |
| Existing applied-CSS audit duration | 3.30 milliseconds |

The whole remaining package predicted **41 → 0** unresolved aliases; the actual result was **0**. All 41 target tokens and selectors passed. Existing provider identities, 213 native fields, owned red/yellow text and yellow tooltip effects, explicit inline overrides and cleanup passed. The four added CSS utility checks passed. The saved receipt also confirms the installed JavaScript, JSON and CSS match the locally checked files. The user confirmed PersonalPins icons look right.

The previous five-repair checkpoint at 23:24:06 UTC reached **41 missing** from 46. Its receipt remains a separate historical result.
The preceding four-repair scan at 22:02:51 UTC reached **46 missing** from 50, covering the menu image and three owned compatibility styles.

The preceding 32-target checkpoint at 21:08:34 UTC reached **50 missing**, with its separate status-editor finder verified against all 15 fields after the editor was opened. The earlier 30-alias checkpoint at 18:35:34 UTC had reached **65 missing**. Their original receipts remain separate; the unchanged status descriptor was not made a new editor-opening requirement for this unrelated four-repair batch.

The non-fallback total measures lookup results. It does not independently certify the semantic UI role of every existing mapping. A full group scan deliberately exercises aliases for features that may not currently be open or used.

Focused checks exercise the production finder, class-module proxy and class/selector getters against captured source fixtures. They cover loaded-module identity before lazy synthesis, unloaded-module results, expected tokens, affected sibling aliases, stale-cache rejection, plausible wrong owners and preservation of the public alias inventory. The preceding 32-target local batch check exercised all 32 targets, 113 known native-token checks and 1,588 sibling aliases in about 2.16 seconds. It also reproduced the old popout cache failure, checked six loaded/unloaded orphan interleavings, and verified that the repaired status fingerprint finds the actual export while preserving its eight public outputs. The temporary probe’s focused checks also passed. The resource JSON preserves the existing formatting; its parsed values and key order are identical to the live-tested data.

The temporary local probe runs only from its settings buttons. Startup and opening or reopening settings do not start a scan. It records pre-lookup snapshots, separates native-export and stylesheet-only evidence, supports cancellation, and reports save failures. This diagnostic helper and its local evidence captures are not part of the production library change.

## Remaining work

The reported public-alias repair set now has **0 unresolved aliases**. Conditional native UI states that were not exercised remain explicit validation limits. The scan also retains pre-lookup group snapshots: 23 historical groups have no public aliases, while 12 temporarily unavailable groups resolve their aliases through existing lazy suffixes. These snapshot counts do not represent 35 missing public aliases or verified native export identities.

Future Discord revisions still require source and runtime comparison. Keep this temporary layer until equivalent upstream fixes are verified, then remove the corresponding fork changes.

For every manual test, report:

1. The current unresolved count, intended repairs and expected remaining count **before** testing.
2. The actual remaining count, expected-token results and regressions **after** the probe finishes.

A missing descriptor, export or search result is not sufficient proof that a hook was removed. Conversely, a matching property name or non-fallback lazy string is not sufficient proof of the correct UI owner. Removed or merged states, unresolved owners and verified replacements are kept distinct. Existing aliases will not be made to appear repaired by assigning unrelated classes or inventing native class tokens.

The upstream README and ownership remain unchanged. This file records the fork’s temporary delta and should be updated as additional repairs are accepted or upstream equivalents replace them.
