# Fork changes

Last updated: **2026-10-28** (Australia/Melbourne).

This fork carries temporary BDFDB compatibility fixes for changes in Discord’s frontend classes. The aim is to preserve BDFDB’s existing class aliases and the plugins that consume them through targeted descriptor and mapping changes.

Upstream remains [mwittrien/BetterDiscordAddons](https://github.com/mwittrien/BetterDiscordAddons). These changes are maintained on this fork’s `develop` branch until equivalent upstream fixes are available.

## Branches and scope

| Branch | Purpose | Pull / push convention |
| --- | --- | --- |
| `upstream-master` | Clean upstream comparison and recovery base | Pull from `upstream/master`; push to `origin/upstream-master` |
| `develop` | Fork-specific compatibility work | Pull and push through `origin/develop` |

The current comparison base is upstream commit `819bd99b00cc7870b7a82905ba0a950ca4ee228e`. The clean branch was renamed from `master` to `upstream-master` locally and on the fork, and is the fork’s default branch. The explicit name avoids confusing a local branch with the remote-tracking ref `upstream/master`.

This checkpoint records partial compatibility work maintained on develop. The remaining unresolved aliases are listed by count below; the upstream comparison branch stays clean.

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

## Class compatibility changes

The production mapping changes are in [Library/_res/0BDFDB.data.json](Library/_res/0BDFDB.data.json). All **1,620 existing public alias names** are preserved. The accepted checkpoint contains **73 data entries or field changes** and restores **69 distinct affected aliases** across the areas below.

| Area | Change and verified behavior |
| --- | --- |
| Badges | Match the current `numberBadge` / `iconBadge` export without requiring the removed `textBadge` property. Restores seven base, icon, numeric and shape aliases. The old `badgetextbadge` alias remains unresolved. |
| Image modal | Add verified lazy suffixes for `ModalCarouselNav` and `ImageModalMediaWrapper`, restoring the three navigation aliases and the media wrapper before their modules have loaded. |
| Animations | Add the current `Animations` suffix for the four mapped animation classes. |
| Screen sharing | Remove the obsolete `gradientTop` descriptor requirement and use the current `CallScreenshare` suffix for the three controls aliases. |
| BDFDB changelog | Point `_bdchangelogtitle` to the existing `BD.bdChangelogTitle` class. |
| Menu slider | Point `menuslider` to the current native `Slider.slider` root. The separate legacy slider-container alias remains unresolved. |
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


### Ownership and state boundaries

Several checks prevent a returned class string from being mistaken for a correct repair:

- The old `ModalLayer` fingerprint accepted a Checkpoint/year-in-review stylesheet with `layer` and `hidden` properties. Its `hidden__6b0e6` class belongs to that feature. The descriptor now identifies the current global `layer` / `inactive` owner and rejects the stale cached Checkpoint match. `modallayerhidden` remains unresolved.
- The profile banner mapping identifies the inner element carrying `backgroundImage` / `backgroundColor`, rather than the separate outer frame. It uses BDFDB’s existing lazy mechanism without an ambiguous `banner`-only discovery predicate. Its synthesized class map is not reported as discovery of a Webpack export.
- The status root is the current **status region**. The same class also wraps the self-profile **Add Status** prompt, so matching it does not prove that a nonempty status is present. The hidden reference/measurement container is a separate class.
- The current status emoji class covers both solo and text-plus-emoji forms. CSS sibling selectors distinguish those states. The legacy solo-emoji alias is not mapped to the general emoji class.
- The shorter `customstatusemoji` / `customstatusicon` aliases have a different legacy owner contract from the displayed profile status. They remain unresolved while the editor’s exact replacement is investigated.
- The arrow transition token is present in Discord’s stylesheet but absent from its native JavaScript export. Diagnostics identify this as a stylesheet-backed utility. It is not evidence that the native arrow renderer applies that utility automatically.

No compatibility stylesheet rules have been added at this checkpoint; [Library/_res/0BDFDB.raw.css](Library/_res/0BDFDB.raw.css) is unchanged. No BetterDiscord core or Discord desktop code is changed by this patch. The inspected desktop client receives the current Discord frontend, so an older desktop shell does not imply frozen frontend class names.

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

The latest accepted manual full scan ran at **2026-10-27 18:35:34 UTC**, on stable frontend build **652418**, revision `5c8490b77d210a433203192c6e08fb7cd45550c8`:

| Check | Result |
| --- | ---: |
| Groups covered | 303 |
| Unique aliases covered | 1,620 |
| Aliases returning non-fallback strings | 1,555 |
| Aliases still returning missing-class fallbacks | 65 |
| Getter errors / undefined results / malformed mappings | 0 |
| Newly missing aliases in that batch | 0 |
| Scan duration | 29.46 seconds |

The latest 30-alias batch predicted **95 → 65** unresolved aliases; the actual result was **65**, with all 30 expected tokens matching through both `disCN` and `dotCN`. The report recorded the build/version metadata; a separate whitelisted Console check supplied the main asset `web.33a8a649a2fda558.js` after the probe’s metadata limit missed that filename. The original report is preserved and the supplemental receipt binds its exact bytes and tested data.

The non-fallback total measures lookup results. It does not independently certify the semantic UI role of every existing mapping. A full group scan deliberately exercises aliases for features that may not currently be open or used.

Focused checks exercise the production finder, class-module proxy and class/selector getters against captured source fixtures. They cover loaded-module identity before lazy synthesis, unloaded-module results, expected tokens, affected sibling aliases, stale-cache rejection, plausible wrong owners and preservation of the public alias inventory. The final batch check exercised all 30 replacements, 73 known native-token checks and 1,590 sibling aliases in about 0.43 seconds. The temporary probe’s focused checks also passed. The resource JSON preserves the existing formatting; its parsed values and key order are identical to the live-tested data.

The temporary local probe runs only from its settings buttons. Startup and opening or reopening settings do not start a scan. It records pre-lookup snapshots, separates native-export and stylesheet-only evidence, supports cancellation, and reports save failures. This diagnostic helper and its local evidence captures are not part of the production library change.

## Remaining work

The remaining **65 aliases** are under investigation. Verified replacements will be consolidated into batches of **roughly 30 repaired aliases per manual test**, while preserving the full remaining-alias audit scope. Changed state representations and unresolved owners do not count as repaired aliases.

For every manual test, report:

1. The current unresolved count, intended repairs and expected remaining count **before** testing.
2. The actual remaining count, expected-token results and regressions **after** the probe finishes.

A missing descriptor, export or search result is not sufficient proof that a hook was removed. Conversely, a matching property name or non-fallback lazy string is not sufficient proof of the correct UI owner. Removed or merged states, unresolved owners and verified replacements are kept distinct. Existing aliases will not be made to appear repaired by assigning unrelated classes or inventing native class tokens.

The upstream README and ownership remain unchanged. This file records the fork’s temporary delta and should be updated as additional repairs are accepted or upstream equivalents replace them.
