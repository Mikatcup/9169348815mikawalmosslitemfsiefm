# MossLight — Complete Project Plans and Task History

Generated: 2026-08-29T19:41:24.000Z
Records: 274

This is a point-in-time export of every authoritative project-task record. It intentionally includes unfinished, cancelled, merged, proposed, pending, and any other recorded lifecycle states.

## Status summary

| Status | Count |
|---|---:|
| MAIN_PENDING | 1 |
| PROPOSED | 27 |
| MERGED | 179 |
| CANCELLED | 67 |

## All plans

### MAIN_PENDING (1)

#### #270 — Make Apple sign-in available in TestFlight
- **Status:** PENDING
- **Created:** 2026-08-29T13:27:56.886Z
- **Updated:** 2026-08-29T14:50:34.655Z
- **Artifact kinds:** mobile

# Restore Apple Sign-In

## What & Why
Apple sign-in is still unavailable in the TestFlight build even though prior setup work was marked complete. Repair the Replit-managed Clerk Apple connection and enable the native button only after the full flow is proven.

## Done looks like
- “Sign in with Apple” is enabled on supported iOS devices and no longer labeled Coming soon.
- New users can create a MossLight account with Apple and reach the correct garden/onboarding flow.
- Existing users can sign in and recover the correct account without duplicate identities or garden loss.
- Cancellation and provider errors return safely to sign-in with a clear, non-destructive message.
- Hidden-email Apple accounts work correctly.
- The complete flow is verified in a production-profile TestFlight build before release.

## Out of scope
- Replacing Clerk as the authentication provider.
- Adding Apple sign-in to Android.
- Changing Google or email sign-in except where shared account-linking behavior requires protection.

## Steps
1. **Confirm provider state** -- Inspect the Replit-managed Clerk production configuration and determine why the native app still treats Apple as unavailable.
2. **Repair the Apple connection** -- Configure the supported Apple OAuth connection, redirect behavior, identifiers, and native entitlements without exposing credentials.
3. **Enable guarded native UI** -- Show the functional button only on supported iOS builds and retain safe cancellation/error handling.
4. **Protect account continuity** -- Verify Apple’s private-relay email and identity-linking cases cannot create an unintended second garden.
5. **Validate on TestFlight** -- Test create-account, returning sign-in, cancellation, error, and hidden-email flows in a production-profile iOS build.

## Relevant files
- `artifacts/mobile/app/(auth)/sign-in.tsx`
- `artifacts/mobile/app.config.ts`
- `artifacts/mobile/context/GardenContext.tsx`
- `artifacts/mobile/utils/api.ts`

---

### PROPOSED (27)

#### #69 — Show a toast confirmation when a companion friend is toggled on
- **Status:** PROPOSED
- **Created:** 2026-05-23T02:13:10.857Z
- **Updated:** 2026-05-23T02:13:10.857Z
- **Depends on:** #68
- **Proposed from:** #68
- **Category:** next_steps

# Show a toast when a companion friend is toggled on

  ## What & Why
  The Garden Friends section lets users toggle companion reminders on/off, but enabling one gives no feedback beyond the visual checkbox. A brief toast ("Mossy the Frog added as your garden friend!") would round out the consistent toast pattern now used across all other profile actions.

  ## Done looks like
  - Toggling a friend ON shows a toast with the friend's name
  - Toggling OFF does not show a toast (deactivation is self-evident from the UI)
  - Toast style matches the existing pattern in `artifacts/mobile/app/(tabs)/profile.tsx`

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — friendToggle onPress handlers (lines ~470-495)

---

#### #70 — Let users undo an accidental tier upgrade
- **Status:** PROPOSED
- **Created:** 2026-05-23T02:13:10.857Z
- **Updated:** 2026-05-23T02:13:10.857Z
- **Depends on:** #68
- **Proposed from:** #68
- **Category:** next_steps

# Let users undo an accidental tier upgrade

  ## What & Why
  Once a tier upgrade is confirmed via the Alert dialog and the toast fires, there's no way to reverse it. Adding a brief "Undo" action button on the success toast (similar to common undo-toast patterns) would protect users from accidental taps.

  ## Done looks like
  - The upgrade success toast includes a tappable "Undo" label
  - Tapping Undo within the toast window reverts the tier to the previous value via a new `downgradeTier` (or equivalent) method in GardenContext
  - If the toast expires without tapping Undo, the upgrade is finalized

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — handleUpgrade, showToast
  - `artifacts/mobile/context/GardenContext.tsx` — upgradeTier, cottageTier state

---

#### #79 — Show a banner when purchased skins are restored after reinstall
- **Status:** PROPOSED
- **Created:** 2026-05-28T21:43:10.384Z
- **Updated:** 2026-05-28T21:43:10.384Z
- **Depends on:** #77
- **Proposed from:** #77
- **Category:** next_steps

# Show a banner when purchased skins are restored after reinstall

  ## What & Why
  When a user reinstalls the app and their purchased premium skins are synced back from the cloud, there's no indication that restoration happened. A brief confirmation banner would reassure users that their purchases were recovered.

  ## Done looks like
  - When the cloud hydration step restores a non-empty purchasedPremiumSkins list to a fresh install (i.e., local storage had none), a toast or banner appears saying something like "Your purchased skins have been restored"
  - The banner only fires once per install, not on every subsequent app open

  ## Relevant files
  - `artifacts/mobile/context/GardenContext.tsx` — hydration block around line 603-604 where purchasedPremiumSkins is restored from the cloud snapshot

---

#### #80 — Restore equipped skin automatically when switching devices
- **Status:** PROPOSED
- **Created:** 2026-05-28T21:43:10.384Z
- **Updated:** 2026-05-28T21:43:10.384Z
- **Depends on:** #77
- **Proposed from:** #77
- **Category:** next_steps

# Restore equipped skin automatically when switching devices

  ## What & Why
  The activeSkins map (which skin is currently equipped per friend type) is synced to the cloud, but it can reference a skinId that hasn't been purchased yet on the new device. The merge logic should validate that any restored activeSkins entries correspond to an owned (unlocked or purchased) skin before applying them.

  ## Done looks like
  - After cloud hydration, any activeSkins entries that reference a premium skinId not present in purchasedPremiumSkins are silently cleared
  - The user sees no equipped skin rather than a broken/invisible skin on a fresh install

  ## Relevant files
  - `artifacts/mobile/context/GardenContext.tsx` — hydration block around lines 603-604 where activeSkins and purchasedPremiumSkins are both restored

---

#### #97 — Show free skin descriptions in the milestone progress screen
- **Status:** PROPOSED
- **Created:** 2026-05-29T00:39:36.639Z
- **Updated:** 2026-05-29T00:39:36.639Z
- **Depends on:** #90
- **Proposed from:** #90
- **Category:** next_steps

# Show free skin descriptions in the milestone progress screen

  ## What & Why
  Free milestone skins (Hornworm, Golden Ladybug, Luna Moth, Emerald Beetle) now have flavour descriptions in SKIN_UNLOCKS, but the milestone/progress UI where users see which skins they're working toward doesn't surface those descriptions yet. Showing them there gives users more motivation to keep earning points.

  ## Done looks like
  - The milestone progress list (wherever free skin milestones are displayed) renders the description beneath the skin name
  - Style matches the premium skin preview sheet description rendering

  ## Relevant files
  - `artifacts/mobile/context/GardenContext.tsx` — SKIN_UNLOCKS with new description fields
  - Search for where SKIN_UNLOCKS is consumed in the milestone/progress UI

---

#### #99 — Show the earned-skins restored toast on any screen, not just Profile
- **Status:** PROPOSED
- **Created:** 2026-05-29T00:40:34.808Z
- **Updated:** 2026-05-29T00:40:34.808Z
- **Depends on:** #87
- **Proposed from:** #87
- **Category:** next_steps

# Show the earned-skins restored toast on any screen, not just Profile

  ## What & Why
  The free-skins restored notification (task #87) fires from a useEffect in profile.tsx — the same limitation the premium skin toast had before task #82 fixed it. If the user lands on a different tab after reinstall, they miss the notification entirely.

  ## Done looks like
  - `freeSkinsRestoredCount` and `clearFreeSkinsRestoredNotification` are consumed from a global or layout-level component (e.g. the root layout or a shared ToastManager) so the toast fires regardless of which tab is active
  - Follows the same pattern used to fix the premium skin toast (task #82)

  ## Relevant files
  - `artifacts/mobile/context/GardenContext.tsx` — freeSkinsRestoredCount state
  - `artifacts/mobile/app/(tabs)/profile.tsx` — current toast trigger location
  - `artifacts/mobile/app/_layout.tsx` — likely home for a global toast listener

---

#### #109 — Apply the same diagonal-scroll guard to the free skin carousel
- **Status:** PROPOSED
- **Created:** 2026-05-29T01:15:33.352Z
- **Updated:** 2026-05-29T01:15:33.352Z
- **Depends on:** #96
- **Proposed from:** #96
- **Category:** incomplete_scope

# Apply the same diagonal-scroll guard to the free skin carousel

  ## What & Why
  Task #96 hardened the premium skin preview carousel against accidental swipes triggered by diagonal scrolling. The free skin carousel (milestone skins) uses a similar pan responder and is exposed to the same problem.

  ## Done looks like
  - The free skin carousel's `onMoveShouldSetPanResponder` requires `|dx| > 2 * |dy|`
  - Its flick detection in `onPanResponderRelease` rejects flicks where `|vy|` is comparable to `|vx|` (e.g. `|vx| > 1.5 * |vy|`)
  - Diagonal page-scroll gestures no longer accidentally change the free skin

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — look for the free skin carousel pan responder (separate from `previewPanResponder`)

---

#### #116 — Re-enable the iOS 26 liquid-glass tab bar once the upstream fix lands
- **Status:** PROPOSED
- **Created:** 2026-05-29T02:26:22.433Z
- **Updated:** 2026-05-29T02:26:22.433Z
- **Depends on:** #100
- **Proposed from:** #100
- **Category:** tech_debt

# Re-enable the iOS 26 liquid-glass tab bar once the upstream fix lands

  ## What & Why
  The liquid-glass (NativeTabs) tab bar was hardcoded off in `useLiquidGlass()` to stop a native-level crash on iOS 26.x caused by `expo-router/unstable-native-tabs` initialising its native module. The crash bypasses both try/catch and React ErrorBoundary, so the only safe fix was to prevent the component from rendering at all.

  When Expo ships a stable release of `expo-router/unstable-native-tabs` that doesn't crash on iOS 26, this guard should be removed so iOS 26+ users get the native liquid-glass tab bar.

  ## Done looks like
  - `useLiquidGlass()` in `artifacts/mobile/app/(tabs)/_layout.tsx` is restored to call `isLiquidGlassAvailable()` (remove the hardcoded `return false`)
  - Tested on a physical iOS 26 device via TestFlight with no crash
  - The `NativeTabFallback` ErrorBoundary wrapper can be kept as a belt-and-suspenders guard

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — `useLiquidGlass()` function (search for "TODO: re-enable")

---

#### #119 — Keep the last-visited highlight when tapping a skin, not just on reopen
- **Status:** PROPOSED
- **Created:** 2026-05-29T02:53:07.389Z
- **Updated:** 2026-05-29T02:53:07.389Z
- **Depends on:** #102
- **Proposed from:** #102
- **Category:** next_steps

# Keep the last-visited highlight when tapping a skin, not just on reopen

  ## What & Why
  Currently the highlight only appears when the profile screen reopens and scrolls to the last-tapped row. Tapping the Equip button on a skin doesn't immediately show the highlight ring — users who stay on the screen won't see feedback that the "last visited" marker moved to the row they just tapped.

  ## Done looks like
  - Tapping Equip on a free skin row triggers the same fade-in/fade-out highlight animation on that row immediately, without needing to leave and reopen the screen

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — onPress handler at ~line 793, freeSkinHighlightOpacity animation at ~line 297

---

#### #134 — Animate dot color smoothly when switching skins
- **Status:** PROPOSED
- **Created:** 2026-05-29T05:51:44.299Z
- **Updated:** 2026-05-29T05:51:44.299Z
- **Depends on:** #117
- **Proposed from:** #117
- **Category:** next_steps

# Animate dot color smoothly when switching skins

  ## What & Why
  The dot width and scale now spring-animate when scrubbing, but the dot background color (active: foreground, inactive: border) still switches instantly. A short color interpolation would make the full transition feel cohesive.

  ## Done looks like
  - Each dot's background color cross-fades using an Animated.Value-driven interpolation
  - The color transition runs in sync with the existing width/scale spring (~120ms feel)
  - No regressions on tap-to-jump or scrub behavior

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — dotWidthAnims, dotScaleAnims, dot rendering (~line 1666)

---

#### #135 — Apply the same smooth spring animation to the free-skin carousel dots
- **Status:** PROPOSED
- **Created:** 2026-05-29T05:51:44.299Z
- **Updated:** 2026-05-29T05:51:44.299Z
- **Depends on:** #117
- **Proposed from:** #117
- **Category:** next_steps

# Apply the same smooth spring animation to the free-skin carousel dots

  ## What & Why
  The premium skin picker dots now animate smoothly with spring transitions. The free-skin carousel has its own dot indicator that still snaps instantly. Applying the same pattern there would make both carousels feel consistent.

  ## Done looks like
  - Free-skin carousel dots animate width and scaleY with Animated.spring on index change
  - Matches the premium dot spring config (damping 20, stiffness 260)
  - No regressions on free-skin swipe or tap-to-jump behavior

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — freeSkinCarouselIndex, free skin dot rendering

---

#### #136 — Show the tooltip at the finger position instead of centered
- **Status:** PROPOSED
- **Created:** 2026-05-29T06:18:36.197Z
- **Updated:** 2026-05-29T06:18:36.197Z
- **Depends on:** #118
- **Proposed from:** #118
- **Category:** next_steps

# Show the tooltip at the finger position instead of centered

  ## What & Why
  The current scrub tooltip always centers itself above the dot bar. Following the user's finger horizontally (like a native slider) makes it feel more native and precise — especially when scrubbing across many skins.

  ## Done looks like
  - The tooltip X position tracks locationX from the touch event during scrub
  - It stays clamped within the panel so it never clips off the edge
  - The label still fades in/out on scrub start/end

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — dotBarTouchHandlers, scrubTooltip, dotBarWidthRef

---

#### #137 — Extend scrub tooltips to the free-skin dot bar too
- **Status:** PROPOSED
- **Created:** 2026-05-29T06:18:36.197Z
- **Updated:** 2026-05-29T06:18:36.197Z
- **Depends on:** #118
- **Proposed from:** #118
- **Category:** next_steps

# Extend scrub tooltips to the free-skin dot bar too

  ## What & Why
  The premium-skin dot bar now shows a name tooltip during scrub, but the free-skin carousel has a similar dot indicator with no such feedback. Parity between the two carousels is cleaner and more consistent.

  ## Done looks like
  - A matching tooltip appears above the free-skin dot row during scrub (if scrub is implemented there)
  - Shows `freeSkins[freeSkinCarouselIndex].label`
  - Fades in/out in sync with the scrub gesture

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — freeSkinCarouselIndex, freeSkins, freeSkinContentX

---

#### #138 — Sync automatically as soon as the internet comes back
- **Status:** PROPOSED
- **Created:** 2026-05-29T06:22:34.101Z
- **Updated:** 2026-05-29T06:22:34.101Z
- **Depends on:** #122
- **Proposed from:** #122
- **Category:** incomplete_scope

# Sync automatically as soon as the internet comes back

  ## What & Why
  Task #122 makes failed syncs retry the next time the user opens the app (foreground transition). But if the app is already open in the foreground when connectivity returns — say the user is browsing plants with wifi off and then reconnects — the retry doesn't fire until they background and re-open.

  Adding a network-state listener would close this gap: the sync would flush the pending-sync flag the moment connectivity is restored, even mid-session.

  ## Done looks like
  - When the device regains network connectivity while the app is in the foreground, any pending sync (PENDING_SYNC_KEY = "1" in AsyncStorage) is automatically retried
  - No manual foreground/background cycle needed
  - Expo SDK 54 ships expo-network (~7.0.x) which provides a useNetworkState hook — this is the cleanest integration path without adding a new native dependency

  ## Relevant files
  - `artifacts/mobile/context/GardenContext.tsx` — PENDING_SYNC_KEY, forceSyncRef, AppState listener (lines 686–700)
  - `artifacts/mobile/utils/api.ts` — pushProfile, ProfileSyncError
  - Install `expo-network` (already included in SDK 54 bundled modules, may just need adding to package.json)

---

#### #139 — Make the celebration burst feel themed to the skin that was just unlocked
- **Status:** PROPOSED
- **Created:** 2026-05-29T07:20:15.751Z
- **Updated:** 2026-05-29T07:20:15.751Z
- **Depends on:** #124
- **Proposed from:** #124
- **Category:** next_steps

# Make the celebration burst feel themed to the skin that was just unlocked

  ## What & Why
  The burst now uses different random angles, sizes, and colors on every unlock — but the color palette is always the same set (gold, parchment, sage, white, etc.). Using colors sampled from the specific unlocked skin's artwork would make each unlock feel personal and connected to the reward.

  ## Done looks like
  - Each skin definition (or at least the milestone free skins) has 2–3 brand colors associated with it
  - CelebrationBurst receives a `colors` prop drawn from the unlocked skin and uses those instead of (or blended with) PARTICLE_COLORS
  - The burst visually echoes the skin that was just unlocked

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — CelebrationBurst, PARTICLE_COLORS, buildParticles()
  - `artifacts/mobile/context/GardenContext.tsx` — newlyUnlockedSkin (has label, emoji, skinId)
  - `artifacts/mobile/data/` — wherever skin metadata lives; skinId can be used to look up colors

---

#### #140 — Tint the screen flash to match the unlocked skin's color palette
- **Status:** PROPOSED
- **Created:** 2026-05-29T14:05:02.671Z
- **Updated:** 2026-05-29T14:05:02.671Z
- **Depends on:** #125
- **Proposed from:** #125
- **Category:** next_steps

# Tint the screen flash to match the unlocked skin's color palette

  ## What & Why
  The current screen flash always uses a warm parchment-white (#fffbe8). If each skin had 1–2 associated brand colors, the flash could be tinted to match the unlocked skin, making the moment feel even more personal — the screen literally "lights up" in that skin's color.

  ## Done looks like
  - Each skin definition (or milestone skin entry) has an associated highlight/flash color
  - ScreenFlash receives a `color` prop and uses it as backgroundColor
  - The flash color changes per unlock event, echoing the specific skin earned

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — ScreenFlash, MilestoneSkinsToast
  - `artifacts/mobile/context/GardenContext.tsx` — newlyUnlockedSkin (skinId, label, emoji)
  - `artifacts/mobile/data/` — wherever skin metadata lives

---

#### #142 — Scroll the profile screen to the skin section when arriving from the unlock toast
- **Status:** PROPOSED
- **Created:** 2026-05-29T14:29:42.175Z
- **Updated:** 2026-05-29T14:29:42.175Z
- **Depends on:** #126
- **Proposed from:** #126
- **Category:** next_steps

# Scroll Profile to the skin section on toast-tap navigation

  ## What & Why
  When the user taps the unlock toast they land on the Profile tab, but they could be scrolled anywhere on the page. The highlight animation plays at the free-skin carousel, which may be off-screen. Adding an automatic scroll ensures the reward is always immediately visible.

  ## Done looks like
  - After `requestSkinHighlight` fires and the profile renders, the ScrollView scrolls to the skin carousel section before (or timed with) the highlight animation
  - Works whether Profile was already mounted in the background or freshly mounting
  - Does not interfere with the existing `triggerSkinHighlight` highlight/jump logic

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — `triggerSkinHighlight`, `pendingHighlightSkinId` useEffect, the ScrollView ref
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — `MilestoneSkinsToast`, `handlePress`

---

#### #143 — Add a haptic tap when the unlock toast is pressed
- **Status:** PROPOSED
- **Created:** 2026-05-29T14:29:42.175Z
- **Updated:** 2026-05-29T14:29:42.175Z
- **Depends on:** #126
- **Proposed from:** #126
- **Category:** next_steps

# Haptic feedback on unlock toast tap

  ## What & Why
  The unlock toast is already tappable, but there is no haptic response when the user taps it. Adding a light impact haptic makes the interaction feel snappy and confirms the tap registered, matching the polish level of other interactive elements in the app.

  ## Done looks like
  - `Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Medium)` (or similar) fires at the start of `handlePress` in `MilestoneSkinsToast`
  - Does not fire if `skinId` is null (guard already exists)

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — `MilestoneSkinsToast`, `handlePress` (~line 215)

---

#### #144 — Re-schedule notifications on a new device when notification preferences arrive from sync
- **Status:** PROPOSED
- **Created:** 2026-05-29T14:33:50.566Z
- **Updated:** 2026-05-29T14:33:50.566Z
- **Depends on:** #123
- **Proposed from:** #123
- **Category:** next_steps

# Re-schedule notifications on a new device when notification preferences arrive from sync

  ## What & Why
  Notification enabled/hour preferences (`@quietgrove:notifEnabled`, `@quietgrove:notifHour`) are now included in the cloud sync snapshot. But applying a snapshot on a new device only writes the raw values to AsyncStorage — it does not re-schedule the actual iOS local notifications. A user who had notifications on their old device will see the correct toggle state restored, but no notification will actually fire until they open Settings and toggle the preference themselves.

  ## Done looks like
  - When `applySnapshot` restores notification prefs to a new device, the notification scheduling function is called automatically
  - If notifEnabled is "1" in the snapshot, the notification is scheduled for the stored hour
  - If notifEnabled is "0", any existing scheduled notification is cancelled
  - Works silently in the background with no visible prompt to the user

  ## Relevant files
  - `artifacts/mobile/utils/api.ts` — `applySnapshot` (where the fix should go, or called immediately after)
  - `artifacts/mobile/context/GardenContext.tsx` — notification scheduling logic (setNotificationsEnabled, setNotificationHour)

---

#### #145 — Clean up per-user conflict preference keys when a user signs out
- **Status:** PROPOSED
- **Created:** 2026-05-29T14:33:50.566Z
- **Updated:** 2026-05-29T14:33:50.566Z
- **Depends on:** #123
- **Proposed from:** #123
- **Category:** tech_debt

# Clean up per-user conflict preference keys on sign-out

  ## What & Why
  The conflict resolution preference is stored as `@quietgrove:conflict_pref:{userId}` — one key per user ID per device. When a user signs out, the key for that user is not cleared, so it accumulates indefinitely on shared devices. Over time a single device could build up conflict_pref keys for many user accounts.

  ## Done looks like
  - On sign-out, the conflict_pref key for the current user (`@quietgrove:conflict_pref:{userId}`) is removed from AsyncStorage
  - The removal is added to the wipe list in `clearLocalState` or the sign-out flow in GardenContext

  ## Relevant files
  - `artifacts/mobile/context/GardenContext.tsx` — `clearLocalState` (~line 1468)
  - `artifacts/mobile/app/(auth)/sign-in.tsx` — CONFLICT_PREF_KEY (`@quietgrove:conflict_pref`)
  - `artifacts/mobile/app/(tabs)/profile.tsx` — `conflictPrefKey` (~line 319)

---

#### #146 — Show the unlock toast again if the app was closed before the user saw it
- **Status:** PROPOSED
- **Created:** 2026-05-29T15:03:15.631Z
- **Updated:** 2026-05-29T15:03:15.631Z
- **Depends on:** #127
- **Proposed from:** #127
- **Category:** next_steps

# Show the unlock toast again if the app was closed before the user saw it

  ## What & Why
  The "New skin unlocked" toast is driven by the in-memory `newlyUnlockedSkin` state in GardenContext. If the app is killed (crash, swipe-close) between the unlock firing and the toast actually showing on screen, the notification is gone forever — the user never knows they earned something. Task #127 fixed the *highlight* surviving restarts; the *toast* itself still doesn't.

  ## Done looks like
  - When `newlyUnlockedSkin` is set, write the payload (label, emoji, skinId) to AsyncStorage under `@quietgrove:pendingUnlockToast`
  - On launch, if the key exists and the skin is actually in the user's unlocked set, re-surface the toast once
  - Clear the key when the toast is shown or dismissed
  - `clearLocalState()` removes the key on sign-out

  ## Relevant files
  - `artifacts/mobile/context/GardenContext.tsx` — `newlyUnlockedSkin`, `clearNewlyUnlockedSkin` (lines ~1445–1460)
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — renders the toast, calls `clearNewlyUnlockedSkin` (line ~220)

---

#### #166 — Make sure the app looks great for users who need larger text or have other accessibility needs
- **Status:** PROPOSED
- **Created:** 2026-05-30T13:36:20.475Z
- **Updated:** 2026-05-30T13:36:20.475Z
- **Depends on:** #162
- **Proposed from:** #162
- **Category:** next_steps

# Accessibility support (VoiceOver, Larger Text, Dark Interface, Reduced Motion, Differentiate Without Color)

  ## What & Why
  Task #165 is already pending for this. Skipping duplication.

  ## Done looks like
  - See task #165

---

#### #211 — Catch broken password recovery before users are locked out
- **Status:** PROPOSED
- **Created:** 2026-08-23T22:02:25.829Z
- **Updated:** 2026-08-23T22:02:25.829Z
- **Depends on:** #206
- **Proposed from:** #206
- **Category:** test_gaps

# Catch broken password recovery before users are locked out

## What & Why
Password recovery now runs entirely inside MossLight. A focused test with Clerk test accounts will catch configuration or SDK regressions that could otherwise prevent people from regaining access to their gardens.

## Done looks like
- The email sign-in form exposes the password recovery link
- A Clerk test account can receive and use a reset code, set a new password, and return to an active session
- Invalid and expired codes, unknown emails, and weak passwords show inline errors without native alerts
- The flow is checked on both iOS and Android

## Relevant files
- artifacts/mobile/app/(auth)/sign-in.tsx

---

#### #213 — Catch garden-choice regressions before email sign-in ships
- **Status:** PROPOSED
- **Created:** 2026-08-23T22:22:41.787Z
- **Updated:** 2026-08-23T22:22:41.787Z
- **Depends on:** #207
- **Proposed from:** #207
- **Category:** test_gaps

# Catch garden-choice regressions before email sign-in ships

## What & Why
Email sign-in now protects a guest garden when the account already has plants in the cloud. This flow has several destructive branches, so an automated regression check would make sure a future auth change cannot silently overwrite the device garden or apply one user's saved choice to another account.

## Done looks like
- A guest with real local plants and an account with cloud plants is shown a choice before either garden is changed
- Merge, keep-device, and use-cloud choices produce the expected garden and persist only for the signed-in account
- A saved choice for one account is not used for a different account on the same device

## Relevant files
- artifacts/mobile/app/(auth)/sign-in.tsx
- artifacts/mobile/components/MergePreviewSheet.tsx
- artifacts/mobile/context/GardenContext.tsx

---

#### #220 — Keep the browser Herbarium preview from losing its API data
- **Status:** PROPOSED
- **Created:** 2026-08-24T00:25:12.045Z
- **Updated:** 2026-08-24T00:25:12.045Z
- **Depends on:** #217
- **Proposed from:** #217
- **Category:** test_gaps

# Keep the browser Herbarium preview from losing its API data

## What & Why
The browser preview depends on the API accepting the Expo preview origin. A small origin or routing regression can otherwise leave the Herbarium and Learn screens without their remote catalog, which makes browser checks misleading.

## Done looks like
- An automated browser smoke check opens the Expo preview and reaches the Herbarium screen
- The check confirms species content and Learn topic content render after entering the guest flow
- The check reports a failed or blocked catalog request clearly

## Relevant files
- artifacts/api-server/src/app.ts
- artifacts/mobile/lib/plantCatalog.ts
- artifacts/mobile/context/PlantCatalogContext.tsx
- artifacts/mobile/app/(tabs)/discover.tsx

---

#### #230 — Catch browser garden and catalog regressions before releases
- **Status:** PROPOSED
- **Created:** 2026-08-25T06:05:22.755Z
- **Updated:** 2026-08-25T06:05:22.755Z
- **Depends on:** #229
- **Proposed from:** #229
- **Category:** test_gaps

# Catch browser garden and catalog regressions before releases

## What & Why
The browser experience now has local garden mutations, journal persistence, accessibility preferences, mounted-path catalog images, and exact plant-detail lookups. A repeatable end-to-end check will prevent a change to the API response, browser storage, or routing prefix from silently breaking those core flows.

## Done looks like
- A browser test starts at the mounted MossLight preview route and verifies catalog loading, image rendering, search, and an ambiguous exact-ID detail such as rose
- The test adds, edits, cares for, and removes a local plant, then verifies reload persistence
- The test creates, edits, and deletes an associated journal entry and verifies local persistence
- The test covers Learn progress and the persisted theme/larger-text preferences
- The failure/retry catalog state is covered without relying on production data

## Relevant files
- artifacts/mosslight-preview/src/hooks/use-garden.tsx
- artifacts/mosslight-preview/src/hooks/use-plants.ts
- artifacts/mosslight-preview/src/pages/plants.tsx
- artifacts/mosslight-preview/src/pages/plant-detail.tsx
- artifacts/mosslight-preview/src/pages/journal.tsx
- artifacts/mosslight-preview/src/pages/profile.tsx
- artifacts/api-server/src/routes/plants.ts

---

#### #244 — Catch welcome and guided-tour regressions before releases
- **Status:** PROPOSED
- **Created:** 2026-08-28T03:59:48.860Z
- **Updated:** 2026-08-28T03:59:48.860Z
- **Depends on:** #243
- **Proposed from:** #243
- **Category:** test_gaps

# Catch welcome and guided-tour regressions before releases

## What & Why
The new first-run experience relies on device-local state and a transient replay request. Automated coverage would prevent future navigation or persistence changes from making the welcome reappear, hiding the replay action, or reviving retired overlays.

## Done looks like
- Fresh guest and signed-in entry paths verify the welcome appears once
- Taking, skipping, and completing the tour persist the expected device-local state
- Profile replay always starts at step one and leaves name, account, and garden data unchanged
- Source checks prevent screen-specific TourCard and OnboardingTooltip mounts from returning
- Compact web/Expo smoke coverage confirms the primary and skip controls remain reachable

## Relevant files
- artifacts/mobile/app/_layout.tsx
- artifacts/mobile/components/WelcomeScreen.tsx
- artifacts/mobile/components/FeatureTour.tsx
- artifacts/mobile/app/(auth)/sign-in.tsx
- artifacts/mobile/app/(tabs)/profile.tsx
- artifacts/mobile/context/GardenContext.tsx

---

### MERGED (178)

#### #1 — Fix 3 dependency vulnerabilities
- **Status:** MERGED
- **Created:** 2026-05-22T03:31:14.978Z
- **Updated:** 2026-05-22T03:38:35.973Z

Fix the following dependency vulnerabilities:

- [High] uuid@3.4.0 (GHSA-w5hq-g745-h8pq@uuid-3.4.0)
- [High] uuid@7.0.3 (GHSA-w5hq-g745-h8pq@uuid-7.0.3)
- [Medium] postcss@8.4.49 (GHSA-qx2v-qp2m-jg93@postcss-8.4.49)

---

#### #2 — Security scan
- **Status:** MERGED
- **Created:** 2026-05-22T03:47:35.756Z
- **Updated:** 2026-05-22T03:58:58.526Z

Run an in-depth scan across the entire project

---

#### #3 — Security scan
- **Status:** MERGED
- **Created:** 2026-05-22T07:03:12.964Z
- **Updated:** 2026-05-22T07:12:56.870Z

Run an in-depth scan across the entire project

---

#### #4 — Profile Sync Access Control
- **Status:** MERGED
- **Created:** 2026-05-22T07:12:51.910Z
- **Updated:** 2026-05-22T07:24:10.720Z

Vulnerabilities in the cross-device profile sync flow and its trust boundary.

Vulnerabilities to fix:

1. [High] Unauthenticated profile sync exposes user data behind guessable greenhouse codes
  Anyone who learns or guesses a user's greenhouse code can read or overwrite that user's synced garden profile without logging in. This exposes private in-app data such as notes and journal content, and it also lets attackers corrupt or replace a victim's saved state.

The production API exposes `GET /api/profile/:code` and `PUT /api/profile/:code` with no authentication or authorization checks (`artifacts/api-server/src/routes/profile.ts:9-61`). The server is globally reachable and additionally enables permissive CORS for all origins (`artifacts/api-server/src/app.ts:28`), so any website can issue browser requests directly to these endpoints. The only gate is the `:code` path parameter, validated only against `/^[A-Z]+-\d{4}$/`.

That code is not a secret credential. The client generates it locally from a fixed list of 10 words plus 4 decimal digits (`artifacts/mobile/utils/share.ts:6-12`), giving only about 90,000 possible values. The app also presents the code on the profile screen and explicitly encourages users to share it (`artifacts/mobile/app/(tabs)/profile.tsx:628-653`). During normal app use, the code is then used as the sole identifier for profile backup/restore and background sync (`artifacts/mobile/context/GardenContext.tsx:326-367`, `artifacts/mobile/utils/api.ts:54-79`).

Because the API returns the full stored snapshot, a successful guess yields the victim's synced AsyncStorage-backed data, including plant inventory, notes, journal entries, preferences, and other profile state (`artifacts/mobile/utils/api.ts:7-39`). Because `PUT /api/profile/:code` performs an unauthenticated upsert, the same guess also lets an attacker overwrite or poison the profile that a fresh install restores later (`artifacts/mobile/context/GardenContext.tsx:326-346`). This is a direct broken access control issue with both confidentiality and integrity impact.

A realistic attack is straightforward: script the 10 known words with `0000-9999`, call `GET /api/profile/:code`, and collect every `200 OK` response. A second-stage attack can then write malicious or empty snapshots back to the discovered codes, causing victims to lose or import attacker-controlled data the next time they sync or restore. Clerk sign-in on the mobile client does not mitigate this because the server never binds the profile record to the authenticated user or validates a server-side identity token before servicing the profile route.
  Files: artifacts/api-server/src/app.ts, artifacts/api-server/src/routes/profile.ts, artifacts/mobile/utils/share.ts, artifacts/mobile/utils/api.ts, artifacts/mobile/context/GardenContext.tsx, artifacts/mobile/app/(tabs)/profile.tsx

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #5 — Security scan
- **Status:** MERGED
- **Created:** 2026-05-22T11:55:16.760Z
- **Updated:** 2026-05-22T12:10:37.965Z

Run an in-depth scan across the entire project

---

#### #6 — Fix js-cookie vulnerability
- **Status:** MERGED
- **Created:** 2026-05-22T11:57:43.140Z
- **Updated:** 2026-05-22T12:10:42.703Z

Fix the following dependency vulnerabilities:

- [High] js-cookie@3.0.5 (GHSA-qjx8-664m-686j@js-cookie-3.0.5)

---

#### #7 — Fix Clerk web blank screen
- **Status:** MERGED
- **Created:** 2026-05-22T12:00:31.217Z
- **Updated:** 2026-05-22T12:52:28.120Z

# Fix Clerk Web Blank Screen

## What & Why
The Mosslight app renders a permanent blank white screen on web after Clerk auth was integrated. Two bugs introduced by the Clerk integration are the cause:

1. `tokenCache` from `@clerk/expo/token-cache` uses `expo-secure-store` internally, which does not exist on web. Passing it to `ClerkProvider` on web causes Clerk to hang during initialization and never reach a "loaded" state.

2. `ClerkLoaded` wraps the entire app tree. Since Clerk never finishes loading (due to issue #1), `ClerkLoaded` renders nothing — resulting in a permanent blank white screen.

## Done looks like
- The app renders correctly on web (no blank screen)
- Clerk works on native as before (tokenCache still used on iOS/Android)
- Signed-out users are redirected to the sign-in screen on all platforms
- No regressions in the auth flow

## Out of scope
- Changes to the sign-in UI or auth flow logic
- Any backend or API changes

## Steps
1. **Conditionally apply tokenCache** — In `_layout.tsx`, import `Platform` from `react-native` and only pass `tokenCache` to `ClerkProvider` when `Platform.OS !== 'web'` (pass `undefined` on web).

2. **Replace `ClerkLoaded` with an `isLoaded` guard** — Remove the `ClerkLoaded` wrapper. In `AppShell`, use the `isLoaded` flag from `useAuth()` and return `null` while Clerk is still initializing. This keeps the same blank-while-loading behavior but recovers once Clerk finishes loading (which it now will on web).

## Relevant files
- `artifacts/mobile/app/_layout.tsx`
- `artifacts/mobile/app/(auth)/_layout.tsx`

---

#### #8 — Profile Sync Issues
- **Status:** MERGED
- **Created:** 2026-05-22T12:10:33.275Z
- **Updated:** 2026-05-22T12:22:39.788Z

Vulnerabilities in the production profile backup and restore design, including identifier trust and storage abuse in the `/api/profile/:code` sync path.

Vulnerabilities to fix:

1. [High] Predictable greenhouse codes let any user lock out profile sync
  Any signed-in user can reserve the small set of possible greenhouse codes before other people do, which prevents those victims from ever backing up or restoring their synced profile data. Because the app silently ignores these failed sync attempts, affected users may believe their data is protected when it is not.

The mobile client generates sync identifiers in `randomCode()` from only 10 hard-coded words and a 4-digit suffix (`artifacts/mobile/utils/share.ts:6-12`), yielding just 90,000 possible codes. The backend stores profiles under `profiles.code` as the primary key (`lib/db/src/schema/profiles.ts:3-8`) and treats possession of an unused code as enough authority to create ownership of that row (`artifacts/api-server/src/routes/profile.ts:46-77`). On first write, `PUT /api/profile/:code` checks whether the code already exists, and if it does not, inserts a new row bound to the caller's `userId`. Subsequent callers with the same code are blocked with `403` (`artifacts/api-server/src/routes/profile.ts:59-68`).

This lets an attacker with an ordinary Clerk account script 90,000 authenticated `PUT` requests and pre-claim the entire client-generated code space. After that, every newly installed app instance will eventually generate a code that is already owned by the attacker, so `GET /api/profile/:code` returns `403` instead of a restoreable profile and `PUT /api/profile/:code` also returns `403`, permanently breaking backup/restore for those users. The impact is amplified by the client behavior in `pushProfile()` and `fetchProfile()`: failed writes are ignored and failed reads collapse to `null` (`artifacts/mobile/utils/api.ts:54-88`), so the user receives no warning that sync has been denied.

The write path also has a time-of-check/time-of-use race: the ownership check happens before `insert(...).onConflictDoUpdate(...)`, and the conflict handler unconditionally rewrites `userId` and `snapshot` (`artifacts/api-server/src/routes/profile.ts:70-76`). If two different accounts race to claim the same not-yet-created code, the later conflict update can still take ownership. The fundamental issue is that a small, user-generated identifier is being used as both the database key and the authorization boundary for a private profile.
  Files: artifacts/api-server/src/routes/profile.ts, artifacts/mobile/utils/share.ts, artifacts/mobile/utils/api.ts, lib/db/src/schema/profiles.ts

2. [Medium] Authenticated users can create unlimited persistent profile rows
  A normal user account can keep creating new server-side profile records without any quota or ownership limit, allowing one attacker to consume database storage and degrade the sync service for everyone else. This is a persistent denial-of-service issue because each malicious request leaves another row behind in PostgreSQL.

`PUT /api/profile/:code` accepts any code matching `/^[A-Z]+-\d{4}$/` (`artifacts/api-server/src/routes/profile.ts:8,46-52`). Unlike the client, which only generates a tiny fixed vocabulary, the server allows an effectively unbounded namespace because the alphabetic prefix has no length limit. For each previously unseen code, the route inserts a new row into `profiles` with attacker-controlled JSON in `snapshot` (`artifacts/api-server/src/routes/profile.ts:70-76`). The table schema has no per-user uniqueness, quota, or row-count constraint (`lib/db/src/schema/profiles.ts:3-8`), and the API layer adds no rate limiting or server-side bounds beyond Express's default JSON parser (`artifacts/api-server/src/app.ts:47-55`).

An attacker with one valid bearer token can therefore automate requests such as `AAAA-0001`, `AAAB-0001`, or longer prefixes, each with a near-limit JSON body, to create an arbitrary number of durable rows. Because these writes are persisted in PostgreSQL and there is no cleanup path, the attack can steadily consume storage, increase index and table size, and slow or break future sync operations for legitimate users. The issue is exploitable even if the attacker never targets another user's code directly: the route itself provides a write-any-new-key primitive with no economic or technical guardrails.
  Files: artifacts/api-server/src/routes/profile.ts, artifacts/api-server/src/app.ts, lib/db/src/schema/profiles.ts

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #9 — Make sure sign-in works smoothly on web browsers
- **Status:** MERGED
- **Created:** 2026-05-22T12:45:50.727Z
- **Updated:** 2026-05-22T12:59:50.291Z
- **Depends on:** #7
- **Proposed from:** #7
- **Category:** test_gaps

# Make sure sign-in works smoothly on web browsers

  ## What & Why
  The Clerk web blank screen fix has not been smoke-tested in a real browser session end-to-end. Verifying the full sign-in → redirect → tabs flow on web catches any remaining edge cases (e.g. OAuth redirects, token refresh).

  ## Done looks like
  - User can open the app on web, see the sign-in screen (not a blank page)
  - Signing in redirects correctly to the tabs view
  - Signing out redirects back to sign-in
  - Native (iOS/Android) auth flow still works as before

  ## Relevant files
  - `artifacts/mobile/app/_layout.tsx`
  - `artifacts/mobile/app/(auth)/_layout.tsx`
  - `artifacts/mobile/app/(auth)/sign-in.tsx`

---

#### #10 — Let users sign out and land back on the sign-in screen correctly on web
- **Status:** MERGED
- **Created:** 2026-05-22T12:56:34.362Z
- **Updated:** 2026-05-22T13:07:40.579Z
- **Depends on:** #9
- **Proposed from:** #9
- **Category:** next_steps

# Let users sign out and land back on the sign-in screen correctly on web

  ## What & Why
  Sign-in to the app now works smoothly on web. The sign-out flow should be smoke-tested end-to-end on web to confirm that signing out redirects back to the sign-in screen (not a blank page or a broken state).

  ## Done looks like
  - Tapping sign-out on web redirects back to the sign-in screen without a blank page
  - Native (iOS/Android) sign-out still works as before
  - No console errors during the sign-out redirect

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — checks auth state, handles redirect
  - `artifacts/mobile/app/(auth)/_layout.tsx` — redirects signed-in users away from auth screens
  - `artifacts/mobile/app/_layout.tsx` — root Clerk provider and shell

---

#### #11 — Show a smoother loading screen while the app starts up on web
- **Status:** MERGED
- **Created:** 2026-05-22T12:56:34.362Z
- **Updated:** 2026-05-22T13:11:59.267Z
- **Depends on:** #9
- **Proposed from:** #9
- **Category:** next_steps

# Show a smoother loading screen while the app starts up on web

  ## What & Why
  The current loading state while Clerk initializes shows a plain spinner on a dark background. On web, a more polished loading screen (branded, animated, with the MossLight logo) would match the quality of the sign-in screen and feel intentional rather than a fallback.

  ## Done looks like
  - While Clerk loads on web, users see the MossLight logo and a subtle animation instead of a bare spinner
  - The transition from loading to sign-in screen is smooth (no flash)
  - Native splash screen behavior is unchanged

  ## Relevant files
  - `artifacts/mobile/app/_layout.tsx` — the `<ClerkLoading>` block (currently renders a plain ActivityIndicator)

---

#### #12 — Let guest users sign in without losing their plants
- **Status:** MERGED
- **Created:** 2026-05-22T13:04:41.823Z
- **Updated:** 2026-05-22T13:33:17.528Z
- **Depends on:** #10
- **Proposed from:** #10
- **Category:** next_steps

# Let guest users sign in without losing their plants

  ## What & Why
  Users who start in guest mode build up a garden locally. When they sign out and then sign in with Google/Apple, their local plants are silently lost — there's no migration step. This is a high-churn moment.

  ## Done looks like
  - When a guest taps "Sign in" on the sign-in screen (or signs in after being a guest), the app detects locally stored plants and prompts to merge or discard them
  - If merged, local plants are upserted into the user's cloud account
  - The guest-to-authenticated transition is smooth with no data loss

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx` — SSO flow entry point
  - `artifacts/mobile/context/GardenContext.tsx` — local plant state
  - `artifacts/mobile/app/_layout.tsx` — AppShell, handles auth transitions

---

#### #13 — Keep users signed in across browser refreshes on web
- **Status:** MERGED
- **Created:** 2026-05-22T13:04:41.823Z
- **Updated:** 2026-05-22T13:43:59.060Z
- **Depends on:** #10
- **Proposed from:** #10
- **Category:** tech_debt

# Keep users signed in across browser refreshes on web

  ## What & Why
  On web, Clerk is initialized with `tokenCache: undefined` (correct for web), but there is no explicit session persistence configuration. On some browsers, refreshing the page may drop the session cookie, sending the user back to the sign-in screen. This should be verified and hardened.

  ## Done looks like
  - Refreshing the web app while signed in keeps the user signed in
  - Clerk's session cookie/storage is correctly configured for web
  - No blank intermediate screens during session rehydration on page load

  ## Relevant files
  - `artifacts/mobile/app/_layout.tsx` — ClerkProvider setup (tokenCache, proxyUrl)
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — auth gate logic

---

#### #14 — Add a smooth fade-out when the loading screen disappears
- **Status:** MERGED
- **Created:** 2026-05-22T13:10:22.877Z
- **Updated:** 2026-05-22T13:52:20.308Z
- **Depends on:** #11
- **Proposed from:** #11
- **Category:** next_steps

# Add a smooth fade-out when the loading screen disappears

  ## What & Why
  The branded loading screen fades in nicely, but when Clerk finishes loading the screen is simply unmounted — no fade-out. Adding an exit animation would make the transition from loading to sign-in feel seamless rather than abrupt.

  ## Done looks like
  - The `BrandedLoadingScreen` fades out (opacity 1 → 0) before unmounting
  - The sign-in screen appears underneath as the loading screen fades, creating a cross-fade effect
  - No visible flash or jump between the two states

  ## Relevant files
  - `artifacts/mobile/components/BrandedLoadingScreen.tsx` — the loading screen component
  - `artifacts/mobile/app/_layout.tsx` — where `<ClerkLoading>` / `<ClerkLoaded>` are rendered; will need a shared state/ref to coordinate the exit timing

---

#### #15 — Polish the loading experience on slow connections with a timeout message
- **Status:** MERGED
- **Created:** 2026-05-22T13:10:22.877Z
- **Updated:** 2026-05-22T13:55:28.993Z
- **Depends on:** #11
- **Proposed from:** #11
- **Category:** next_steps

# Polish the loading experience on slow connections with a timeout message

  ## What & Why
  On very slow or flaky connections, Clerk can take several seconds to initialize. The branded loading screen shows indefinitely with no feedback. Adding a gentle "taking longer than usual…" message after a few seconds would reassure users rather than leaving them wondering if the app is broken.

  ## Done looks like
  - After ~5 seconds in `<ClerkLoading>`, a soft message appears below the dots (e.g. "Taking a moment — hang tight")
  - The message fades in gently so it doesn't feel alarming
  - The message disappears naturally once loading completes

  ## Relevant files
  - `artifacts/mobile/components/BrandedLoadingScreen.tsx` — add a timed state that shows the fallback message

---

#### #16 — Let returning users merge cloud plants when signing in on a new device
- **Status:** MERGED
- **Created:** 2026-05-22T13:17:18.619Z
- **Updated:** 2026-05-22T14:06:43.290Z
- **Depends on:** #12
- **Proposed from:** #12
- **Category:** next_steps

# Let returning users merge cloud plants when signing in on a new device

  ## What & Why
  The current guest-to-authenticated migration (task #12) handles the case where a guest user signs in for the first time. But if a user has an existing cloud account, signs out, uses the app as a guest again, then signs back in — the cloud plants and the new guest plants both exist and neither is automatically merged. The user should be offered a conflict resolution step.

  ## Done looks like
  - When a user signs in and BOTH local plants AND cloud plants exist, they are prompted to choose: "Merge both gardens", "Keep my local plants", or "Use my cloud garden"
  - "Merge both" upserts local plants into the cloud account without duplicating plants that share the same ID
  - The UX is friendly and clearly explains what each option does

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx` — SSO flow, guest detection added in task #12
  - `artifacts/mobile/context/GardenContext.tsx` — `forceSyncToCloud`, `discardLocalPlants` added in task #12
  - `artifacts/mobile/utils/api.ts` — `fetchProfile`, `pushProfile`

---

#### #17 — Show a confirmation before discarding guest plants
- **Status:** MERGED
- **Created:** 2026-05-22T13:17:18.619Z
- **Updated:** 2026-05-22T14:13:55.742Z
- **Depends on:** #12
- **Proposed from:** #12
- **Category:** next_steps

# Show a confirmation before discarding guest plants

  ## What & Why
  When a user taps "Start fresh" in the guest migration prompt introduced in task #12, their local plants are immediately discarded. There is no second chance. This is a destructive action that could frustrate users who tapped the wrong button.

  ## Done looks like
  - Tapping "Start fresh" shows a second confirmation: "Are you sure? Your X locally saved plants will be removed."
  - The confirmation has a "Yes, start fresh" (destructive) and "Go back" option
  - If the user goes back, they return to the original merge/discard prompt

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx` — the Alert.alert flow for guest migration

---

#### #18 — Keep guest plants safe when the app restarts unexpectedly
- **Status:** MERGED
- **Created:** 2026-05-22T13:34:51.232Z
- **Updated:** 2026-05-22T14:21:11.975Z
- **Depends on:** #13
- **Proposed from:** #13
- **Category:** tech_debt

# Keep guest plants safe when the app restarts unexpectedly

  ## What & Why
  The guest mode flag (`@quietgrove:guest_mode`) is read asynchronously from AsyncStorage on every tab layout mount. If AsyncStorage is slow or unavailable, the app briefly shows a loading screen and could theoretically redirect a guest user to sign-in. The guest state should be cached in React context alongside auth state to make the guest check instant and reliable.

  ## Done looks like
  - Guest mode state is initialized once (in `GardenContext` or a dedicated `AuthContext`) and made available synchronously to any component that needs it
  - The AsyncStorage read in `(tabs)/_layout.tsx` is removed in favor of the context value
  - No blank/loading flash for guest users on app resume

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/_layout.tsx`
  - `artifacts/mobile/app/_layout.tsx`
  - `artifacts/mobile/context/GardenContext.tsx`

---

#### #19 — Let the loading screen fade in on native (iOS/Android) too
- **Status:** MERGED
- **Created:** 2026-05-22T13:45:28.420Z
- **Updated:** 2026-05-22T14:44:38.784Z
- **Depends on:** #14
- **Proposed from:** #14
- **Category:** next_steps

# Let the loading screen fade in on native (iOS/Android) too

  ## What & Why
  The `BrandedLoadingScreen` component returns null immediately on native (`if (Platform.OS !== "web") return null;`), so iOS and Android users see a plain dark background during Clerk initialization with no branding. The new `LoadingOverlay` infrastructure now exists in `_layout.tsx` and already wraps a plain dark view for native — this is the natural next step to give native users the same polished branded experience.

  ## Done looks like
  - `BrandedLoadingScreen` renders the logo, script text, and pulsing dots on native, not just web
  - The fade-out on dismiss works on native with `useNativeDriver: true` where possible
  - No crash or layout issue on iOS or Android

  ## Relevant files
  - `artifacts/mobile/components/BrandedLoadingScreen.tsx` — remove the web-only guard and ensure native-compatible styles
  - `artifacts/mobile/app/_layout.tsx` — `LoadingOverlay` already renders for native; no structural changes needed

---

#### #20 — Animate the sign-in screen sliding up as the loading screen fades away
- **Status:** MERGED
- **Created:** 2026-05-22T13:45:28.420Z
- **Updated:** 2026-05-22T14:48:35.409Z
- **Depends on:** #14
- **Proposed from:** #14
- **Category:** next_steps

# Animate the sign-in screen sliding up as the loading screen fades away

  ## What & Why
  The loading screen now cross-fades out smoothly, but the content underneath (sign-in screen) simply appears in place with no motion of its own. A subtle slide-up entrance on the auth screen would complete the transition and give the whole launch sequence a cohesive, premium feel.

  ## Done looks like
  - The sign-in / auth screen slides up (translateY offset → 0) as the loading overlay fades out
  - The timing is coordinated so the slide starts just before the overlay fully disappears
  - No jarring jump or flash

  ## Relevant files
  - `artifacts/mobile/app/(auth)/_layout.tsx` or the main auth screen component — add entrance animation
  - `artifacts/mobile/app/_layout.tsx` — `LoadingOverlay` fade duration is 500 ms (Easing.out ease)

---

#### #21 — Show the timeout message on the native loading screen too
- **Status:** MERGED
- **Created:** 2026-05-22T13:53:23.814Z
- **Updated:** 2026-05-22T14:55:04.774Z
- **Depends on:** #15
- **Proposed from:** #15
- **Category:** incomplete_scope

# Show the timeout message on the native loading screen too

  ## What & Why
  The "Taking a moment — hang tight" timeout message was added only to the web version of BrandedLoadingScreen (guarded by `Platform.OS !== "web"`). Native (iOS/Android) users on slow connections see the same indefinite spinner with no feedback.

  ## Done looks like
  - After ~5 seconds, the same soft timeout message appears on the native loading screen
  - It fades in gently, matching the web behaviour
  - The message disappears once Clerk finishes loading

  ## Relevant files
  - `artifacts/mobile/components/BrandedLoadingScreen.tsx` — the `Platform.OS !== "web"` guard currently skips all of the component on native; the native loading path needs to be identified and updated accordingly

---

#### #22 — Let users preview what a merge will look like before committing
- **Status:** MERGED
- **Created:** 2026-05-22T13:59:27.727Z
- **Updated:** 2026-05-22T15:01:22.592Z
- **Depends on:** #16
- **Proposed from:** #16
- **Category:** next_steps

# Let users preview what a merge will look like before committing

  ## What & Why
  The current "Merge both gardens" option combines local and cloud plants immediately. Users have no way to see what the merged result will look like — they can't tell which plants are new vs duplicates before committing. A preview screen would give them confidence before making a potentially irreversible choice.

  ## Done looks like
  - Before merging, a sheet/modal shows: plants that will be kept from local, plants that will be added from cloud, and any shared IDs that would be deduplicated
  - Users can confirm or go back to choose a different option
  - Consistent with the app's existing sheet/modal patterns

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx` — where the merge dialog is shown
  - `artifacts/mobile/context/GardenContext.tsx` — `mergeLocalWithCloud` function
  - `artifacts/mobile/utils/api.ts` — `fetchProfile`

---

#### #23 — Warn users that 'Use my cloud garden' will remove their local plants
- **Status:** MERGED
- **Created:** 2026-05-22T13:59:27.727Z
- **Updated:** 2026-05-22T15:03:18.891Z
- **Depends on:** #16
- **Proposed from:** #16
- **Category:** next_steps

# Warn users that 'Use my cloud garden' will remove their local plants

  ## What & Why
  The "Use my cloud garden" option in the conflict dialog discards local plants permanently. Right now it uses React Native's `style: "destructive"` styling but has no secondary confirmation. Users who tap it by accident lose their local plants with no undo.

  ## Done looks like
  - Tapping "Use my cloud garden" triggers a second confirmation: "This will remove {N} local plants. Are you sure?"
  - The warning clearly states the action is permanent
  - Matches the existing confirmation patterns in the app (task for discarding guest plants may already have patterns to follow)

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx` — the "Use my cloud garden" onPress handler

---

#### #24 — Sync conflict resolution for users who sign in on multiple devices simultaneously
- **Status:** MERGED
- **Created:** 2026-05-22T13:59:27.727Z
- **Updated:** 2026-05-22T15:13:30.903Z
- **Depends on:** #16
- **Proposed from:** #16
- **Category:** tech_debt

# Sync conflict resolution for users who sign in on multiple devices simultaneously

  ## What & Why
  The background debounced sync in GardenContext pushes local state to the cloud 5 seconds after any state change. When a merge/restore is in progress (e.g., the user is looking at the conflict dialog), the background sync could fire and overwrite the cloud with stale local data — especially on slow devices or if the dialog sits open for a while.

  ## Done looks like
  - The background sync is paused or cancelled during an active conflict-resolution flow
  - After the user picks an option and the chosen sync completes, the background sync resumes normally
  - No data is overwritten by a race between the dialog action and the 5-second debounce timer

  ## Relevant files
  - `artifacts/mobile/context/GardenContext.tsx` — debounced sync useEffect (~line 374)
  - `artifacts/mobile/app/(auth)/sign-in.tsx` — SSO + conflict resolution flow

---

#### #25 — Also confirm before replacing local plants with the cloud garden
- **Status:** MERGED
- **Created:** 2026-05-22T14:08:39.679Z
- **Updated:** 2026-05-22T15:15:23.692Z
- **Depends on:** #17
- **Proposed from:** #17
- **Category:** next_steps

# Also confirm before replacing local plants with the cloud garden

  ## What & Why
  The "Use my cloud garden" option in the "Two gardens found" alert immediately overwrites local plants with the cloud copy — the same destructive pattern that task #17 fixed for "Start fresh". A user who taps the wrong button loses their local plants with no recovery path.

  ## Done looks like
  - Tapping "Use my cloud garden" shows a second confirmation: "Are you sure? Your X local plants will be replaced by your cloud garden."
  - The confirmation has a "Yes, use cloud garden" (destructive) and "Go back" option
  - "Go back" returns to the original "Two gardens found" prompt

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx` — the "Two gardens found" Alert.alert block (around line 156)

---

#### #26 — Let users undo a 'start fresh' for a short window after signing in
- **Status:** MERGED
- **Created:** 2026-05-22T14:08:39.679Z
- **Updated:** 2026-05-22T15:31:31.178Z
- **Depends on:** #17
- **Proposed from:** #17
- **Category:** next_steps

# Let users undo a 'start fresh' for a short window after signing in

  ## What & Why
  Even with a confirmation, a user who confirms "start fresh" has no recovery if they change their mind. Keeping a temporary local backup (e.g. in AsyncStorage, cleared after 24 h) and offering an "Undo" banner immediately after sign-in would give users a safety net without changing the normal flow.

  ## Done looks like
  - Before discarding, local plants are snapshotted to a temporary AsyncStorage key
  - After sign-in, a dismissible banner appears: "Your local plants were removed. Undo?"
  - Tapping "Undo" restores the snapshot and syncs to cloud
  - The snapshot is automatically cleared after 24 hours

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx` — discardLocalPlants call
  - `artifacts/mobile/context/GardenContext.tsx` — plant state management

---

#### #29 — Animate the sign-in screen sliding up as the loading screen fades away
- **Status:** MERGED
- **Created:** 2026-05-22T14:22:41.322Z
- **Updated:** 2026-05-22T15:41:48.157Z
- **Depends on:** #19
- **Proposed from:** #19
- **Category:** next_steps

# Animate the sign-in screen sliding up as the loading screen fades away

  ## What & Why
  Now that the branded loading screen fades out on both web and native, the transition to the sign-in screen is abrupt. A slide-up entrance on the auth screen would make the handoff feel intentional and polished on all platforms.

  ## Done looks like
  - The sign-in screen animates in (slide up + fade) as the LoadingOverlay fades out
  - Works on iOS, Android, and web
  - No jarring cut between the loading screen and the auth flow

  ## Relevant files
  - `artifacts/mobile/app/_layout.tsx` — LoadingOverlay fade-out timing to coordinate with
  - `artifacts/mobile/app/(auth)/_layout.tsx` or the sign-in screen component

---

#### #30 — Polish the launch feel with a subtle logo fade-in on the loading screen
- **Status:** MERGED
- **Created:** 2026-05-22T14:45:25.911Z
- **Updated:** 2026-05-22T16:26:39.212Z
- **Depends on:** #20
- **Proposed from:** #20
- **Category:** next_steps

# Polish the launch feel with a subtle logo fade-in on the loading screen

  ## What & Why
  The loading screen now fades out smoothly and the auth screen slides up to meet it. The natural next step is giving the loading screen's own content (logo, wordmark) a brief fade-in entrance at app start, making the full launch sequence feel completely intentional from first frame to last.

  ## Done looks like
  - The BrandedLoadingScreen logo/wordmark fades or scales in on mount
  - The entrance is short (~400ms) so it doesn't slow the perceived load time
  - Works on both iOS/Android and web

  ## Relevant files
  - `artifacts/mobile/components/BrandedLoadingScreen.tsx`

---

#### #31 — Apply a matching slide-up entrance to the main tab screen after sign-in
- **Status:** MERGED
- **Created:** 2026-05-22T14:45:25.911Z
- **Updated:** 2026-05-22T19:09:12.442Z
- **Depends on:** #20
- **Proposed from:** #20
- **Category:** next_steps

# Apply a matching slide-up entrance to the main tab screen after sign-in

  ## What & Why
  The auth screen now slides up beautifully when the loading overlay fades away. But after the user signs in and lands on the tabs screen, there's no matching entrance animation — it's a hard cut. Carrying the same motion language through sign-in completion would make the whole flow feel cohesive.

  ## Done looks like
  - The (tabs) layout animates in with a coordinated slide-up or fade on first mount after auth
  - Timing is consistent with the auth entrance (~600ms, ease-out)
  - No jarring jump when transitioning from sign-in to the home tab

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/_layout.tsx`

---

#### #32 — Show the 'hang tight' message when reopening the app after being signed out
- **Status:** MERGED
- **Created:** 2026-05-22T14:49:40.876Z
- **Updated:** 2026-05-22T19:26:43.159Z
- **Depends on:** #21
- **Proposed from:** #21
- **Category:** next_steps

# Show the 'hang tight' message when reopening the app after being signed out

  ## What & Why
  The timeout message now shows on the native loading screen during the auth layout's loading state. However, the root `_layout.tsx` also has a `ClerkLoadingOverlay` component that wraps `BrandedLoadingScreen`. On a slow connection, users who are signed out and re-open the app see the overlay's loading screen first — that path already uses BrandedLoadingScreen which has the timeout built in. Worth verifying the full set of loading entry-points consistently shows the timeout message with no regressions.

  ## Done looks like
  - All loading entry-points (root overlay, auth layout, tabs layout) consistently show the timeout message after ~5 seconds
  - No duplicate timers or conflicting states across paths

  ## Relevant files
  - `artifacts/mobile/app/_layout.tsx` — ClerkLoadingOverlay component
  - `artifacts/mobile/app/(auth)/_layout.tsx` — now uses BrandedLoadingScreen
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — uses BrandedLoadingScreen

---

#### #33 — Fix iOS stuck on green screen at launch
- **Status:** MERGED
- **Created:** 2026-05-22T14:52:29.757Z
- **Updated:** 2026-05-22T19:21:51.896Z

# Fix iOS stuck on green at launch

## What & Why
On iOS (Expo Go), the app never transitions from the base green color to the sign-in screen. The root cause is in `(auth)/_layout.tsx`: while Clerk is initializing (`!isLoaded`), iOS/Android intentionally renders a plain `View` with `backgroundColor: PALETTE.forestDeep`, relying on the root `LoadingOverlay` to cover it. On iOS, the overlay's `StyleSheet.absoluteFillObject` + `zIndex: 999` can fail to position above the screen when the parent provider chain doesn't establish an explicit flex layout context — so the green fallback bleeds through and the user sees nothing but green indefinitely.

## Done looks like
- On iOS, launching the app shows the `BrandedLoadingScreen` (MossLight logo, pulsing dots) while Clerk initializes
- Once Clerk is ready, the sign-in screen slides into view as expected
- No regression on Android or web

## Out of scope
- Root-cause fixing the `LoadingOverlay` z-index behavior (the overlay can stay as an enhancement layer)
- Any changes to the sign-in flow itself

## Steps
1. **Remove the platform fork in the auth layout loading state** — In `(auth)/_layout.tsx`, replace the `Platform.OS === "web"` ternary that returns `<BrandedLoadingScreen />` vs a plain `View`. Return `<BrandedLoadingScreen />` unconditionally for all platforms when `!isLoaded`. Remove the now-unused `View` import from `react-native` if it's no longer needed elsewhere in that file.

## Relevant files
- `artifacts/mobile/app/(auth)/_layout.tsx:32-38`
- `artifacts/mobile/components/BrandedLoadingScreen.tsx`
- `artifacts/mobile/app/_layout.tsx:119-145`

---

#### #39 — Let users go back if they accidentally tap 'Keep these plants' or 'Merge'
- **Status:** MERGED
- **Created:** 2026-05-22T15:14:23.975Z
- **Updated:** 2026-05-22T19:22:16.495Z
- **Depends on:** #25
- **Proposed from:** #25
- **Category:** next_steps

# Let users go back if they accidentally tap 'Keep these plants' or 'Merge'

  ## What & Why
  The "Two gardens found" dialog now guards "Use my cloud garden" with a confirmation, but the other two options ("Keep these plants" and "Merge both gardens") still execute immediately on tap with no way back. Adding confirmations or a brief undo window for those paths makes the conflict flow consistently safe.

  ## Done looks like
  - Tapping "Keep these plants" shows a confirmation before uploading local data to cloud
  - Tapping "Merge both gardens" can be cancelled before the merge is applied
  - All three conflict-resolution options feel equally safe to tap

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx` — the "Two gardens found" alert and merge preview logic (around lines 175–235)

---

#### #40 — Let the undo banner fade out on its own after a minute
- **Status:** MERGED
- **Created:** 2026-05-22T15:18:41.879Z
- **Updated:** 2026-05-22T19:26:45.167Z
- **Depends on:** #26
- **Proposed from:** #26
- **Category:** next_steps

# Let the undo banner fade out on its own after a minute

  ## What & Why
  The "Your local plants were removed. Undo?" banner currently stays visible until the user taps Dismiss or Undo. Auto-dismissing it after ~60 seconds (with a fade animation) would keep the UI clean without requiring any action from users who don't want to undo.

  ## Done looks like
  - The banner starts a countdown on mount (e.g. 60 seconds)
  - When the timer expires, the banner fades out and clears the snapshot
  - Tapping "Undo" or "Dismiss" cancels the timer

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — UndoBanner component

---

#### #41 — Let users undo other accidental actions — like using a cloud garden instead of their local one
- **Status:** MERGED
- **Created:** 2026-05-22T15:18:41.879Z
- **Updated:** 2026-05-22T19:21:46.365Z
- **Depends on:** #26
- **Proposed from:** #26
- **Category:** next_steps

# Let users undo other accidental actions — like using a cloud garden instead of their local one

  ## What & Why
  The "Use my cloud garden" action in the "Two gardens found" conflict flow also permanently replaces local plants, but has no undo path. Extending the snapshot-and-restore pattern to cover this case would give users the same safety net regardless of which destructive path they took.

  ## Done looks like
  - Before restoring from cloud, the local plants are snapshotted to a second temporary key (e.g. `@quietgrove:replaced_by_cloud_snapshot`)
  - After sign-in, the undo banner shows the appropriate message ("Your local plants were replaced by your cloud garden. Undo?")
  - Tapping Undo restores the local snapshot and re-syncs it to cloud

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx` — "Use my cloud garden" confirm handler
  - `artifacts/mobile/context/GardenContext.tsx` — restoreFromCloudSnapshot, discardLocalPlants

---

#### #44 — Animate each tab screen in when switching tabs for the first time
- **Status:** MERGED
- **Created:** 2026-05-22T16:27:33.265Z
- **Updated:** 2026-05-22T20:09:37.238Z
- **Depends on:** #31
- **Proposed from:** #31
- **Category:** next_steps

# Animate each tab screen in when switching tabs for the first time

  ## What & Why
  The tabs layout now slides in smoothly after sign-in, but individual tab screens (Greenhouse, Conservatory, Journal, etc.) still appear as hard cuts when the user taps them for the first time. Adding a subtle per-screen entrance would carry the motion language through the entire experience.

  ## Done looks like
  - Each tab screen fades or slides up gently on first visit
  - Animation is quick (~250ms) so it doesn't feel sluggish during normal navigation
  - Subsequent visits to the same tab have no animation (no re-entrance on tab switch)

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/_layout.tsx`
  - Individual tab screens: `artifacts/mobile/app/(tabs)/index.tsx`, `garden.tsx`, `journal.tsx`, `discover.tsx`, `profile.tsx`

---

#### #45 — Show a 'something went wrong' message if loading takes too long
- **Status:** MERGED
- **Created:** 2026-05-22T19:11:13.627Z
- **Updated:** 2026-05-22T19:58:34.234Z
- **Depends on:** #32
- **Proposed from:** #32
- **Category:** next_steps

# Show a 'something went wrong' message if loading takes too long

  ## What & Why
  The app currently shows "Taking a moment — hang tight" after 5 seconds, but if Clerk or the garden context never resolves (e.g. network completely offline), the user is stuck on the loading screen forever with no actionable message or way out.

  ## Done looks like
  - After a longer timeout (e.g. 20–30 seconds), BrandedLoadingScreen shows a second message like "Having trouble connecting — please check your connection" with an optional retry or dismiss action
  - No regressions to the existing 5-second hang-tight message

  ## Relevant files
  - `artifacts/mobile/components/BrandedLoadingScreen.tsx` — add a second timer/state for the extended timeout message

---

#### #46 — Remove the extra loading overlay now that each screen handles its own loading state
- **Status:** MERGED
- **Created:** 2026-05-22T19:17:28.847Z
- **Updated:** 2026-05-22T19:28:59.508Z
- **Depends on:** #33
- **Proposed from:** #33
- **Category:** tech_debt

# Remove redundant LoadingOverlay from root layout

  ## What & Why
  The root `_layout.tsx` renders a `LoadingOverlay` (zIndex: 999, absoluteFill) that was the primary loading screen before this fix. Now that `(auth)/_layout.tsx` renders `BrandedLoadingScreen` directly when Clerk isn't ready, the overlay is a redundant second layer. Removing it simplifies the render tree and eliminates the iOS z-index edge case entirely rather than just working around it.

  ## Done looks like
  - `LoadingOverlay` component removed from `artifacts/mobile/app/_layout.tsx`
  - `BrandedLoadingScreen` import in `_layout.tsx` removed if no longer needed
  - App still shows the branded loading screen on all platforms at launch

  ## Relevant files
  - `artifacts/mobile/app/_layout.tsx` (lines 119–145)
  - `artifacts/mobile/app/(auth)/_layout.tsx`

---

#### #47 — Show how much time is left before the undo banner disappears
- **Status:** MERGED
- **Created:** 2026-05-22T19:18:44.283Z
- **Updated:** 2026-05-22T20:45:37.169Z
- **Depends on:** #40
- **Proposed from:** #40
- **Category:** next_steps

# Show how much time is left before the undo banner disappears

  ## What & Why
  The banner now auto-dismisses after 60 seconds, but the user has no visual cue that it's about to disappear. A subtle progress indicator (e.g. a thin shrinking bar along the bottom of the banner) would make the countdown visible and reduce surprise when the banner fades.

  ## Done looks like
  - A thin progress bar inside the UndoBanner depletes over 60 seconds using Animated
  - The bar stops animating if the user taps Undo or Dismiss
  - The style matches the existing banner palette (goldLight / forestDeep)

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — UndoBanner component

---

#### #48 — Let users undo accidental merges from the 'Two gardens found' flow
- **Status:** MERGED
- **Created:** 2026-05-22T19:20:21.269Z
- **Updated:** 2026-05-23T01:51:18.173Z
- **Depends on:** #41
- **Proposed from:** #41
- **Category:** next_steps

# Let users undo accidental merges from the 'Two gardens found' flow

  ## What & Why
  The merge path in the "Two gardens found" conflict flow permanently combines local and cloud plants, with no undo. Extending the snapshot-and-restore pattern to cover the merge action would give users a complete safety net for all three destructive paths in that flow (discard local, use cloud, merge).

  ## Done looks like
  - Before merging, the pre-merge local state is snapshotted to a temporary key (e.g. `@quietgrove:pre_merge_snapshot`)
  - After sign-in, the undo banner shows "Your gardens were merged. Undo?" 
  - Tapping Undo restores the pre-merge local plants and re-syncs to cloud

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx` — MergePreviewSheet onConfirm handler (calls mergeLocalWithCloud then forceSyncToCloud)
  - `artifacts/mobile/context/GardenContext.tsx` — mergeLocalWithCloud, snapshot/restore pattern
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — UndoBanner (already handles discard and replaced-by-cloud cases)

---

#### #49 — Remember which garden option the user picked, so they aren't asked again on the same device
- **Status:** MERGED
- **Created:** 2026-05-22T19:21:05.748Z
- **Updated:** 2026-05-22T19:26:53.477Z
- **Depends on:** #39
- **Proposed from:** #39
- **Category:** next_steps

# Remember which garden option the user picked

  ## What & Why
  Right now, every time a conflict is detected the user sees the "Two gardens found" dialog again. If a user intentionally chose "Keep these plants" or "Merge" on a previous sign-in on the same device, they shouldn't have to decide again — we should remember and apply the same resolution automatically.

  ## Done looks like
  - After a user resolves a conflict, their preference is stored locally (e.g. AsyncStorage)
  - On future sign-ins where the same type of conflict is detected, the preference is applied silently or with a brief confirmation banner rather than the full dialog

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx` — conflict detection and resolution logic
  - `artifacts/mobile/context/GardenContext.tsx` — sync/merge utilities

---

#### #50 — Let users reset their saved garden preference from the Settings screen
- **Status:** MERGED
- **Created:** 2026-05-22T19:24:39.231Z
- **Updated:** 2026-05-23T01:56:15.608Z
- **Depends on:** #49
- **Proposed from:** #49
- **Category:** next_steps

# Let users reset their saved garden preference from Settings

  ## What & Why
  Task #49 silently re-applies the user's previous conflict resolution choice on
  future sign-ins. But if a user's situation changes (e.g. they now want to merge
  instead of always keeping local plants), there's no way to clear that preference
  without reinstalling the app.

  A simple "Reset garden preference" option in Settings gives users control without
  exposing technical complexity.

  ## Done looks like
  - Settings screen shows a "Conflict preference" row that displays the current
    saved choice (e.g. "Always keep device plants")
  - Tapping it lets the user clear it; next sign-in shows the full dialog again
  - If no preference is saved, the row is hidden or shows "Ask me each time"

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx` — CONFLICT_PREF_KEY = "@quietgrove:conflict_pref"
  - `artifacts/mobile/app/(tabs)/settings.tsx` (or wherever the settings screen lives)

---

#### #51 — Smooth the gap between the loading screen and sign-in on slow connections
- **Status:** MERGED
- **Created:** 2026-05-22T19:28:23.066Z
- **Updated:** 2026-05-22T19:31:19.013Z
- **Depends on:** #46
- **Proposed from:** #46
- **Category:** tech_debt

# Smooth the gap between loading screen and sign-in on slow connections

  ## What & Why
  Now that (auth)/_layout.tsx owns its own loading state, when Clerk finishes 
  initializing it immediately unmounts <BrandedLoadingScreen /> and renders the 
  sign-in Animated.View (starting at opacity 0). There is a 220ms delay before 
  the fade-in animation starts, during which the screen is momentarily blank. On 
  fast devices this is imperceptible, but on slow connections or older iPhones it 
  can produce a brief flash.

  The fix would be to either:
  - Keep the BrandedLoadingScreen rendered underneath the sign-in Animated.View 
    until the animation completes (via absolute positioning), so there is always 
    something visible, OR
  - Remove the 220ms delay now that the root overlay no longer needs to be 
    overlapped

  ## Done looks like
  - No visible blank flash between loading screen and sign-in screen on any device
  - Transition from loading → sign-in feels seamless

  ## Relevant files
  - `artifacts/mobile/app/(auth)/_layout.tsx`

---

#### #52 — Keep the sign-in screen looking great while the keyboard is open
- **Status:** MERGED
- **Created:** 2026-05-22T19:29:48.361Z
- **Updated:** 2026-05-22T19:34:02.126Z
- **Depends on:** #51
- **Proposed from:** #51
- **Category:** next_steps

# Keep the sign-in screen looking great while the keyboard is open

  ## What & Why
  The sign-in screen uses a slide-up + fade-in animation on entry. On smaller iPhones, when the keyboard opens the layout can be pushed off-screen or overlap content, since the entry animation uses translateY and no keyboard-avoidance logic is applied during or after the transition.

  ## Done looks like
  - Sign-in form stays fully visible and scrollable when the keyboard is open on all device sizes
  - No content is obscured by the keyboard after the entry animation completes

  ## Relevant files
  - `artifacts/mobile/app/(auth)/_layout.tsx`
  - `artifacts/mobile/app/(auth)/sign-in.tsx` (if it exists)

---

#### #53 — Keep the sign-in screen looking great on small screens without a keyboard too
- **Status:** MERGED
- **Created:** 2026-05-22T19:32:56.082Z
- **Updated:** 2026-05-22T19:36:01.661Z
- **Depends on:** #52
- **Proposed from:** #52
- **Category:** next_steps

# Keep the sign-in screen looking great on small screens without a keyboard too

  ## What & Why
  The keyboard-avoidance fix (task #52) introduced a ScrollView to the sign-in screen, which handles the keyboard case well. However, on very small iPhones (SE, mini) the content may still feel cramped in the no-keyboard state because the `top` section uses `flex: 1` with `justifyContent: "center"` and a fixed `paddingTop: 48`. The layout could be made more adaptive by reducing spacing on shorter devices.

  ## Done looks like
  - Sign-in screen looks polished on small-screen devices (iPhone SE, mini) without a keyboard
  - Spacing and font sizes adapt gracefully to the available height
  - No visual clipping or awkward white space

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx`

---

#### #55 — Show a preview of the user's local garden on the sign-in screen
- **Status:** MERGED
- **Created:** 2026-05-22T19:35:07.771Z
- **Updated:** 2026-05-22T19:40:23.296Z
- **Depends on:** #53
- **Proposed from:** #53
- **Category:** next_steps

# Show a preview of the user's local garden on the sign-in screen

  ## What & Why
  Users who have already added plants locally have a reason to sign in — syncing their garden. Showing a small count or preview ("You have 3 plants ready to sync") on the sign-in screen gives them a tangible nudge to create an account, especially on the return visit.

  ## Done looks like
  - If the user has local plants, a subtle indicator appears above the sign-in buttons
  - The message adapts to the plant count (e.g. "3 plants ready to back up")
  - Only shown for users with real local content (not demo plants)

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx`
  - `artifacts/mobile/context/GardenContext.tsx`

---

#### #56 — Animate the garden preview badge onto the sign-in screen
- **Status:** MERGED
- **Created:** 2026-05-22T19:37:11.035Z
- **Updated:** 2026-05-22T19:45:32.363Z
- **Depends on:** #55
- **Proposed from:** #55
- **Category:** next_steps

# Animate the garden preview badge onto the sign-in screen

  ## What & Why
  The garden preview badge ("3 plants ready to back up") appears instantly. A gentle fade-in or slide-up entrance would make it feel more considered and draw the eye naturally — fitting with the sign-in screen's existing entrance animation task.

  ## Done looks like
  - The badge fades or slides in with a short delay after the screen mounts
  - Animation is subtle and respects the app's motion style

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx` (gardenPreviewBanner + gardenPreviewText styles)

---

#### #67 — Show a confirmation when the garden preference is successfully reset
- **Status:** MERGED
- **Created:** 2026-05-23T01:52:43.308Z
- **Updated:** 2026-05-23T02:11:05.602Z
- **Depends on:** #50
- **Proposed from:** #50
- **Category:** next_steps

# Show a confirmation when the garden preference is successfully reset

  ## What & Why
  After a user taps "Reset" in the Conflict preference row, the row simply disappears. A brief toast or inline confirmation ("Preference cleared — you'll be asked next time") would give users confidence the action worked.

  ## Done looks like
  - After clearing the preference, a short-lived toast or banner appears confirming the reset
  - The confirmation is non-blocking and dismisses automatically

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — where the reset action lives (handleClearConflictPref)

---

#### #68 — Show confirmations for other destructive profile actions
- **Status:** MERGED
- **Created:** 2026-05-23T01:57:30.176Z
- **Updated:** 2026-05-23T02:17:03.278Z
- **Depends on:** #67
- **Proposed from:** #67
- **Category:** next_steps

# Show confirmations for other destructive profile actions

  ## What & Why
  The conflict preference reset now shows a toast, but other profile actions (sign out, skin purchases, tier upgrades) only use blocking Alert dialogs. Adding non-blocking toast feedback to successful actions would give a more consistent, polished feel.

  ## Done looks like
  - Actions like tier upgrade success and skin equip show a brief toast confirmation
  - Toasts are consistent in style with the conflict preference reset toast

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — handleUpgrade, handlePurchasePremiumSkin, equipSkin calls

---

#### #71 — Fix qs vulnerability
- **Status:** MERGED
- **Created:** 2026-05-23T02:15:43.464Z
- **Updated:** 2026-05-23T02:18:08.799Z

Fix the following dependency vulnerabilities:

- [Medium] qs@6.15.1 (GHSA-q8mj-m7cp-5q26@qs-6.15.1)

---

#### #72 — Security scan
- **Status:** MERGED
- **Created:** 2026-05-27T22:36:12.111Z
- **Updated:** 2026-05-27T22:47:21.295Z

Run a security scan for changes since the last scan

---

#### #73 — Sync State Isolation
- **Status:** MERGED
- **Created:** 2026-05-27T22:47:26.037Z
- **Updated:** 2026-05-27T22:52:56.512Z

Vulnerabilities caused by device-local sync state and conflict resolution being shared across accounts during sign-in and cloud restore flows.

Vulnerabilities to fix:

1. [Low] Saved cloud conflict preference is not bound to the signed-in account
  A decision about how to resolve a sync conflict is remembered for the whole device, not for a specific account. On a shared phone or tablet, one person's saved choice can silently overwrite or merge another person's cloud garden the next time they sign in.

The sign-in flow stores the user's conflict decision in a single global AsyncStorage key, `@quietgrove:conflict_pref`, when they choose keep-local, use-cloud, or merge (`artifacts/mobile/app/(auth)/sign-in.tsx:255`, `artifacts/mobile/app/(auth)/sign-in.tsx:288`, `artifacts/mobile/app/(auth)/sign-in.tsx:501`). On later sign-ins, if both local and cloud data exist, the app reads that key and auto-applies the stored action without checking which Clerk user originally made the choice (`artifacts/mobile/app/(auth)/sign-in.tsx:184-217`).

This preference is not cleared on sign-out. The profile screen exposes a manual reset action (`artifacts/mobile/app/(tabs)/profile.tsx:113-149`), but `performSignOut()` leaves the key intact (`artifacts/mobile/app/(tabs)/profile.tsx:155-161`). That means the next account to use the device inherits the previous account's sync policy.

An attacker does not need server access to abuse this. A prior device user can set the preference to `keep_local`, leave crafted local garden data on the device, and sign out. When a different user later signs in to an account that already has cloud plants, the app skips the warning dialog and immediately runs `forceSyncToCloud()`, replacing the victim's cloud garden with the device-local copy. The same issue can also silently force `use_cloud` or `merge`, causing unintended data replacement or mixing across accounts. The impact is limited by the requirement for access to the same device, so this is best rated LOW, but it is still a real authorization and data-integrity regression in the new guest-to-account flow.
  Files: artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/app/(tabs)/profile.tsx

2. [Medium] Cloud restore leaves prior user's local data in place and syncs it into the next account
  When one person signs in on a device that still has another person's local app data, choosing the cloud copy does not fully clear the older local data. As a result, private notes and other profile fields from the previous device user can remain on the phone and later be uploaded into the newly signed-in account.

The root cause is that `applySnapshot()` only writes keys whose value is non-null and never removes keys that are absent or explicitly null in the server snapshot (`artifacts/mobile/utils/api.ts:46-57`). The synced profile schema includes more than just plants: `ALL_PROFILE_KEYS` also covers journal entries, theme, last check-in state, read markers, device ID, greenhouse code, and unlock state (`artifacts/mobile/utils/api.ts:7-31`).

The new client conflict-resolution flow calls this partial restore path in multiple production-reachable places. On app hydration, a fresh signed-in install loads the cloud snapshot with `applySnapshot(snap)` (`artifacts/mobile/context/GardenContext.tsx:543-587`). During sign-in conflict handling, selecting "Use my cloud garden" also calls `restoreFromCloudSnapshot()`, which again delegates to `applySnapshot(snap)` (`artifacts/mobile/app/(auth)/sign-in.tsx:197-205`, `artifacts/mobile/app/(auth)/sign-in.tsx:287-295`, `artifacts/mobile/context/GardenContext.tsx:1182-1223`). Neither path clears old local values for keys that the cloud account does not currently have.

That matters because several profile features still read directly from AsyncStorage and therefore keep using the stale values: journal entries (`artifacts/mobile/hooks/useJournal.ts:12-29`), daily check-in state (`artifacts/mobile/hooks/useDailyPrompt.ts:5-31`), and theme selection (`artifacts/mobile/context/ThemeContext.tsx:18-44`). Once background sync or a later manual sync runs, `buildSnapshot()` re-collects every local key from AsyncStorage and `pushProfile()` uploads them back to the server (`artifacts/mobile/utils/api.ts:37-44`, `artifacts/mobile/context/GardenContext.tsx:624-646`, `artifacts/mobile/context/GardenContext.tsx:1032-1039`).

An exploitation scenario is straightforward on a shared device: User A leaves guest or signed-in journal data on the device, signs out, and User B signs in and chooses their cloud garden. If User B's cloud snapshot has `journalEntries`, `lastCheckIn`, or similar fields set to `null`, User A's local values survive the restore and are later synced into User B's cloud profile. This creates cross-account confidentiality and integrity impact even though the bug starts in local storage, because the stale data crosses the trust boundary into server-backed state.
  Files: artifacts/mobile/utils/api.ts, artifacts/mobile/context/GardenContext.tsx, artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/hooks/useJournal.ts, artifacts/mobile/hooks/useDailyPrompt.ts, artifacts/mobile/context/ThemeContext.tsx

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #74 — Fix companion skin bugs (Atlas Moth, hedgehog, Jade Beetle)
- **Status:** MERGED
- **Created:** 2026-05-28T14:45:45.013Z
- **Updated:** 2026-05-28T21:36:43.949Z

# Fix Companion Skin Bugs

## What & Why
Three issues in the companion/skin system need fixing: the Atlas Moth premium skin doesn't auto-equip after purchase (leaving the Luna Moth active), the hedgehog critter asset contains a toad illustration, and the Jade Beetle skin label/image doesn't match user expectations.

## Done looks like
- Purchasing the Atlas Moth skin automatically equips it (replaces whatever was in the `moth` slot), so the atlas moth image appears immediately in the cottage and skin list without a separate "Equip" tap
- The hedgehog companion (`toad` FriendType internally) displays an actual hedgehog illustration matching the existing critter art style
- The Jade Beetle skin is renamed to "Emerald Beetle" with an appropriate green/iridescent beetle image replacing the jade-beetle.png (or confirmed correct if the image already fits), so users see a clearly labelled and visually accurate skin
- Sakura Mantis and Calico Cat premium skin rows are verified visible in the Premium Skins shop section with "Soon" badges and correct preview images

## Out of scope
- Implementing real IAP purchases (still "Soon" / stubbed)
- Changing the plant limit upsell modal (already says "Coming Soon")
- New skins beyond what is listed

## Steps
1. **Auto-equip on premium purchase** — In `handlePurchasePremiumSkin` in `profile.tsx`, after calling `purchasePremiumSkin(skin.skinId)`, also call `equipSkin(skin.skinId)` so the purchased skin is immediately active. Update the toast message accordingly.

2. **Replace hedgehog critter asset** — Generate a new hedgehog illustration (`hedgehog.png`) in the same pixel-art / flat critter style as the existing critters (`butterfly.png`, `frog.png`, etc.) and replace `artifacts/mobile/assets/critters/hedgehog.png`.

3. **Fix Jade Beetle skin** — Rename the `jadebeetle` skin label from "Jade Beetle" to "Emerald Beetle" in `SKIN_UNLOCKS` in `GardenContext.tsx`. Generate a new emerald/iridescent beetle image (or use the existing `dung_beetle.png` if appropriate) and update `critterSkins.ts` to point to the correct asset. Ensure the preview in the skin shop reflects the new image.

4. **Verify Sakura Mantis and Calico Cat** — Confirm both skins appear in the Premium Skins shop section with correct preview images (`sakura-mantis.png`, `calico-cat.png`) and "Soon" badges. Fix any display issues found.

## Relevant files
- `artifacts/mobile/app/(tabs)/profile.tsx:258-274`
- `artifacts/mobile/context/GardenContext.tsx:159-168,780-802`
- `artifacts/mobile/lib/critterSkins.ts`
- `artifacts/mobile/assets/critters/hedgehog.png`
- `artifacts/mobile/assets/critters/jade-beetle.png`
- `artifacts/mobile/assets/critters/sakura-mantis.png`
- `artifacts/mobile/assets/critters/calico-cat.png`

---

#### #76 — Security scan
- **Status:** MERGED
- **Created:** 2026-05-28T20:54:49.918Z
- **Updated:** 2026-05-28T21:41:36.500Z

Run an in-depth scan across the entire project

---

#### #77 — Keep purchased companion skins safe if the app is reinstalled
- **Status:** MERGED
- **Created:** 2026-05-28T21:11:10.539Z
- **Updated:** 2026-05-28T21:54:13.156Z
- **Depends on:** #74
- **Proposed from:** #74
- **Category:** tech_debt

# Keep purchased companion skins safe if the app is reinstalled

  ## What & Why
  Purchased premium skins (Atlas Moth, Scarab Beetle, etc.) are currently stored only in AsyncStorage on the device. If a user reinstalls the app, switches devices, or clears app data they lose the skins they paid $1.99 for — with no way to restore them. These purchases need to be backed up to the cloud profile.

  ## Done looks like
  - Purchased premium skin IDs are persisted to the user's cloud profile (api-server) alongside other profile data
  - On app load / sign-in, the cloud-stored purchased skins are restored and merged with local state
  - The sync flow in GardenContext.tsx treats purchasedPremiumSkins the same way it handles plants and activeSkins

  ## Relevant files
  - `artifacts/mobile/context/GardenContext.tsx` — purchasedPremiumSkinsState, forceSyncToCloud, hydrate logic (~line 795-802, 1100-1200)
  - `artifacts/api-server/src/routes/profile.ts` — profile save/load endpoint
  - `lib/db/src/schema/profiles.ts` — profile schema (add purchasedPremiumSkins column)

---

#### #78 — Let users see a companion skin preview before buying
- **Status:** MERGED
- **Created:** 2026-05-28T21:11:10.539Z
- **Updated:** 2026-05-28T22:01:47.492Z
- **Depends on:** #74
- **Proposed from:** #74
- **Category:** next_steps

# Let users see a companion skin preview before buying

  ## What & Why
  Tapping a premium skin row jumps straight to the purchase alert. Users can't see a full illustration of the skin they're about to buy, which creates hesitation and reduces conversion. A preview modal should show the critter image at readable size with the skin name and price before the purchase confirm.

  ## Done looks like
  - Tapping a premium skin row opens a bottom sheet / modal with the skin's full illustration (from getSkinPreviewSource), name, emoji, price, and a "Buy — $1.99" button
  - The existing Alert.alert purchase flow is triggered from within the modal, not directly from the row tap
  - Works for both IAP_ENABLED=true and false (shows "Coming Soon" state in the modal when IAP is off)

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — handlePurchasePremiumSkin, SKIN_UNLOCKS map (~line 258-545)
  - `artifacts/mobile/lib/critterSkins.ts` — getSkinPreviewSource()

---

#### #81 — Let users swipe between all premium skins in the preview modal
- **Status:** MERGED
- **Created:** 2026-05-28T21:59:52.790Z
- **Updated:** 2026-05-28T23:30:29.312Z
- **Depends on:** #78
- **Proposed from:** #78
- **Category:** next_steps

# Let users swipe between all premium skins in the preview modal

  ## What & Why
  The preview bottom sheet currently shows one skin at a time — users must close it and tap a different row to compare skins. Adding horizontal swipe navigation lets users browse all premium skins without leaving the modal, improving discoverability and reducing friction before purchase.

  ## Done looks like
  - The preview sheet includes left/right chevron buttons (and/or swipe gesture) to jump to the next/previous premium skin
  - The current skin indicator (e.g. dots or counter "2 / 4") is visible
  - Skin content (image, name, emoji, action button) animates when switching

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — previewSheet, openPreview, closePreview, SKIN_UNLOCKS filter for isPremium
  - `artifacts/mobile/lib/critterSkins.ts` — getSkinPreviewSource
  - `artifacts/mobile/context/GardenContext.tsx` — SKIN_UNLOCKS

---

#### #82 — Show a 'Skin Restored' confirmation when purchases are recovered after reinstall
- **Status:** MERGED
- **Created:** 2026-05-28T21:59:52.790Z
- **Updated:** 2026-05-29T00:26:50.203Z
- **Depends on:** #78
- **Proposed from:** #78
- **Category:** next_steps

# Show a 'Skin Restored' confirmation when purchases are recovered after reinstall

  ## What & Why
  When a user reinstalls and their purchased skins are synced back from the cloud, nothing visible happens — the skins just silently reappear. A brief in-app banner or toast confirming "Your premium skins have been restored" would reassure users who reinstalled after a crash or device switch.

  ## Done looks like
  - When hydration detects purchased premium skins in the cloud snapshot on a fresh install (i.e. `purchasedPremiumSkins` transitions from empty to non-empty via cloud sync), a toast or banner fires: "Your [N] premium skin(s) have been restored ✦"
  - Shows once per device, not on every app open

  ## Relevant files
  - `artifacts/mobile/context/GardenContext.tsx` — hydration logic, pushProfile / applySnapshot, purchasedPremiumSkins state
  - `artifacts/mobile/app/(tabs)/profile.tsx` — showToast helper for UI feedback

---

#### #83 — Let users swipe with a finger gesture (not just tap arrows) in the skin preview
- **Status:** MERGED
- **Created:** 2026-05-28T22:05:21.644Z
- **Updated:** 2026-05-29T00:30:46.300Z
- **Depends on:** #81
- **Proposed from:** #81
- **Category:** next_steps

# Let users swipe with a finger gesture in the skin preview

  ## What & Why
  The skin preview modal now has left/right chevron buttons and dot indicators for browsing premium skins, but finger swipe gestures would feel more natural on mobile and are the expected UX for a horizontal carousel.

  ## Done looks like
  - Users can swipe left/right anywhere on the preview sheet content area to navigate between skins
  - The animation follows the finger (rubber-banding) for a native feel
  - Chevron buttons remain as a fallback

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — previewNavRow, previewContentWrap, navigatePreviewSkin, jumpToPreviewSkin
  - Consider using PanResponder (already in react-native) or react-native-gesture-handler (Expo default)

---

#### #84 — Show a short description for each premium skin in the preview
- **Status:** MERGED
- **Created:** 2026-05-28T22:05:21.644Z
- **Updated:** 2026-05-29T00:32:50.029Z
- **Depends on:** #81
- **Proposed from:** #81
- **Category:** next_steps

# Show a short description for each premium skin in the preview

  ## What & Why
  The preview sheet currently shows the skin image, name, emoji, and companion type. Adding a one-line flavour description (e.g. "A mythic scarab adorned in iridescent gold") would make each skin feel more distinct and give users a reason to care before purchasing.

  ## Done looks like
  - Each premium skin entry in SKIN_UNLOCKS has a `description` field
  - The preview sheet renders the description below the companion slot label
  - Text is styled consistently with the existing previewCompanionSlot style

  ## Relevant files
  - `artifacts/mobile/context/GardenContext.tsx` — SKIN_UNLOCKS (SkinUnlock interface + array)
  - `artifacts/mobile/app/(tabs)/profile.tsx` — previewCompanionSlot rendering in the modal

---

#### #85 — Remember which skin tab the user last viewed when they reopen the preview
- **Status:** MERGED
- **Created:** 2026-05-28T22:05:21.644Z
- **Updated:** 2026-05-29T00:37:16.761Z
- **Depends on:** #81
- **Proposed from:** #81
- **Category:** next_steps

# Remember which skin the user last previewed

  ## What & Why
  Currently, tapping any premium skin in the list opens the preview at that exact skin — but navigating away and coming back always resets to the first skin. Restoring the last-viewed index when the sheet reopens would reduce friction for users who compare skins across multiple sessions.

  ## Done looks like
  - The most recently viewed premium skin index is stored (in state or AsyncStorage)
  - When the preview sheet opens via any skin row tap, it still opens at the tapped skin
  - When opened a second time without tapping a specific row (if ever exposed as an entry point), it restores the last index

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — previewSkinIndex, openPreview

---

#### #86 — Show the 'Skins Restored' toast on any screen, not just the Profile tab
- **Status:** MERGED
- **Created:** 2026-05-28T23:32:53.446Z
- **Updated:** 2026-05-29T00:39:52.367Z
- **Depends on:** #82
- **Proposed from:** #82
- **Category:** next_steps

# Show the 'Skins Restored' toast on any screen, not just the Profile tab

  ## What & Why
  The current "Your N premium skins have been restored ✦" toast is wired up inside the Profile tab screen. If the user lands on a different tab first after reinstall (e.g. the Garden tab), they won't see the notification until they navigate to Profile. Moving the toast to a global layout component ensures it fires regardless of which tab is active.

  ## Done looks like
  - The toast (or banner) fires on first render of the root layout, not inside any specific tab
  - The skinsRestoredCount / clearSkinsRestoredNotification context values are consumed in `artifacts/mobile/app/(tabs)/_layout.tsx` or the root `_layout.tsx`
  - Profile tab no longer handles this specific toast itself

  ## Relevant files
  - `artifacts/mobile/context/GardenContext.tsx` — skinsRestoredCount, clearSkinsRestoredNotification
  - `artifacts/mobile/app/(tabs)/profile.tsx` — current toast trigger (to be moved)
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — better home for a global toast trigger

---

#### #87 — Notify users when free companion skins are recovered after reinstall too
- **Status:** MERGED
- **Created:** 2026-05-28T23:32:53.446Z
- **Updated:** 2026-05-29T00:42:57.968Z
- **Depends on:** #82
- **Proposed from:** #82
- **Category:** next_steps

# Notify users when free companion skins are recovered after reinstall too

  ## What & Why
  The restored-skins toast (task #82) only fires for premium skins. Users who earned free skins via Garden Points (Hornworm, Golden Ladybug, Luna Moth, Emerald Beetle) also get their skins silently resynced on reinstall. A confirmation like "Your earned companion skins have been restored" closes that gap.

  ## Done looks like
  - During cloud snapshot hydration on fresh install, detect if activeSkins or free-tier skin unlocks (based on points) transitioned from empty to non-empty
  - A separate or combined toast fires: "Your X earned skin(s) have been restored ✦"
  - Shows once per fresh install, same "once per device" constraint as the premium flow

  ## Relevant files
  - `artifacts/mobile/context/GardenContext.tsx` — hydration block (localIsFresh path), skinsRestoredCount pattern to follow
  - `artifacts/mobile/app/(tabs)/profile.tsx` — showToast helper

---

#### #88 — Add a velocity-based flick to swipe through skins faster
- **Status:** MERGED
- **Created:** 2026-05-29T00:29:09.577Z
- **Updated:** 2026-05-29T00:40:48.202Z
- **Depends on:** #83
- **Proposed from:** #83
- **Category:** next_steps

# Add velocity-based flick gesture to skin preview

  ## What & Why
  The current swipe uses a fixed distance threshold (60px) to decide whether to navigate. A fast flick with a short distance won't trigger a navigation. Adding velocity detection (gestureState.vx) would make the carousel feel more native — a quick flick should always advance the skin even if the finger didn't travel far.

  ## Done looks like
  - A fast horizontal flick (e.g. vx > 0.5) navigates to the next/prev skin even without crossing the 60px threshold
  - Slow, deliberate swipes still require the distance threshold

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — onPanResponderRelease handler in previewPanResponder

---

#### #89 — Let the skin image follow the finger during a swipe (live drag tracking)
- **Status:** MERGED
- **Created:** 2026-05-29T00:29:09.577Z
- **Updated:** 2026-05-29T00:48:01.107Z
- **Depends on:** #83
- **Proposed from:** #83
- **Category:** next_steps

# Animate skin image following the finger during swipe

  ## What & Why
  Currently the content area translates with the finger, but the full-screen image preview doesn't yet show a partial peek of the adjacent skin while dragging. A true carousel feel would show the next/prev skin sliding in from the edge as the user drags.

  ## Done looks like
  - While dragging left, the next skin's image peeks in from the right edge
  - While dragging right, the previous skin's image peeks in from the left edge
  - On release, snaps fully to the target skin or bounces back

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — previewNavRow, previewContentWrap, onPanResponderMove

---

#### #90 — Add descriptions to the free milestone skins too
- **Status:** MERGED
- **Created:** 2026-05-29T00:31:49.598Z
- **Updated:** 2026-05-29T00:42:55.913Z
- **Depends on:** #84
- **Proposed from:** #84
- **Category:** next_steps

# Add descriptions to the free milestone skins too

  ## What & Why
  Premium skins now show a flavour description in the preview sheet, but the free point-milestone skins (Hornworm, Golden Ladybug, Luna Moth, Emerald Beetle) display no description. Filling those in gives every skin the same polished feel and motivates users to grind for points.

  ## Done looks like
  - Each non-premium entry in SKIN_UNLOCKS has a `description` field
  - The free-skin row or any future preview UI for free skins renders that description
  - Descriptions are written in the same one-line flavour style as the premium ones

  ## Relevant files
  - `artifacts/mobile/context/GardenContext.tsx` — SKIN_UNLOCKS array (lines ~163-172)

---

#### #91 — Show skin descriptions in the equipped-skin selector, not just the preview sheet
- **Status:** MERGED
- **Created:** 2026-05-29T00:31:49.598Z
- **Updated:** 2026-05-29T00:44:25.285Z
- **Depends on:** #84
- **Proposed from:** #84
- **Category:** next_steps

# Show skin descriptions in the equipped-skin selector, not just the preview sheet

  ## What & Why
  Descriptions only appear inside the full-screen premium preview modal. Users who already own a premium skin and are just switching which one is equipped never see the description. Surfacing it in the equip row or a small tooltip would reinforce the skin's character at the moment of choice.

  ## Done looks like
  - The skin equip row (or a press-and-hold tooltip) shows the description for owned premium skins
  - Non-premium skins show their description where applicable
  - No layout regression in the existing points card skin list

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — skinRow rendering (around line 615+)
  - `artifacts/mobile/context/GardenContext.tsx` — SKIN_UNLOCKS

---

#### #92 — Remember the last-viewed free skin position too
- **Status:** MERGED
- **Created:** 2026-05-29T00:34:35.753Z
- **Updated:** 2026-05-29T00:56:18.585Z
- **Depends on:** #85
- **Proposed from:** #85
- **Category:** next_steps

# Remember the last-viewed free skin position too

  ## What & Why
  The premium skin preview sheet now remembers the last-viewed index across sessions. The free milestone skins section (rendered as a vertical list in the Garden Points card) has no equivalent memory — extending the same pattern here would give parity and reduce friction for users scrolling through both sections.

  ## Done looks like
  - Scrolling to a specific free skin in the list is remembered across reopens of the profile screen (e.g. scroll position restored via ScrollView ref or tracking last-tapped free skin row)
  - Consistent UX with the premium skins preview persistence

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — free skins list rendered inside the Garden Points card (lines ~615-647)

---

#### #93 — Sync the last-viewed skin across devices
- **Status:** MERGED
- **Created:** 2026-05-29T00:34:35.753Z
- **Updated:** 2026-05-29T00:59:06.903Z
- **Depends on:** #85
- **Proposed from:** #85
- **Category:** next_steps

# Sync the last-viewed skin across devices

  ## What & Why
  The last-viewed premium skin index is now stored in AsyncStorage (device-local). Authenticated users who switch between devices will not see the remembered position carry over. Syncing via the existing cloud garden mechanism would give a consistent experience across devices.

  ## Done looks like
  - Last-viewed skin index is persisted to the cloud garden (API server) alongside other user preferences
  - Switching devices restores the last-viewed skin index in the preview sheet

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — LAST_PREVIEW_SKIN_INDEX_KEY, openPreview, jumpToPreviewSkin
  - `artifacts/api-server/` — cloud garden sync endpoints

---

#### #94 — Show a toast when a free milestone skin is unlocked
- **Status:** MERGED
- **Created:** 2026-05-29T00:38:46.319Z
- **Updated:** 2026-05-29T01:12:29.014Z
- **Depends on:** #86
- **Proposed from:** #86
- **Category:** next_steps

# Show a toast when a free milestone skin is unlocked

  ## What & Why
  Users earn free skins at point milestones (100, 250, 500, 1000 pts), but there's no in-the-moment notification when they cross a threshold — they only discover it by visiting the Profile tab. A global toast at the moment of unlock would make the reward feel satisfying and immediate.

  ## Done looks like
  - GardenContext detects when points cross a milestone that unlocks a new free skin
  - A toast fires from the tab layout (similar to the SkinsRestoredToast added in task #86) regardless of which tab is active
  - The toast message names the skin that was unlocked

  ## Relevant files
  - `artifacts/mobile/context/GardenContext.tsx` — points, SKIN_UNLOCKS, milestone detection
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — where global toasts now live (SkinsRestoredToast pattern)

---

#### #95 — Make fast flicks snap to the skin with a snappier animation
- **Status:** MERGED
- **Created:** 2026-05-29T00:39:32.063Z
- **Updated:** 2026-05-29T01:14:35.527Z
- **Depends on:** #88
- **Proposed from:** #88
- **Category:** next_steps

# Make fast flicks snap to the skin with a snappier animation

  ## What & Why
  Velocity-based flicks now navigate correctly, but the transition animation plays at the same fixed speed (180ms) regardless of how fast the flick was. A fast flick should produce a proportionally faster, snappier slide animation so it feels truly native.

  ## Done looks like
  - The slide-out duration scales with gesture velocity (e.g. faster flick → shorter duration, capped between ~80ms and 180ms)
  - The spring-in after the slide still settles naturally

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — `jumpToPreviewSkin` function, specifically the `Animated.timing` for `outX`

---

#### #96 — Prevent accidental skin navigation when scrolling the page
- **Status:** MERGED
- **Created:** 2026-05-29T00:39:32.063Z
- **Updated:** 2026-05-29T01:54:24.872Z
- **Depends on:** #88
- **Proposed from:** #88
- **Category:** tech_debt

# Prevent accidental skin navigation when scrolling the page

  ## What & Why
  The flick velocity threshold is based on `vx` alone. A diagonal scroll gesture with a moderate horizontal velocity component could trigger an unintended skin navigation. The pan responder should more aggressively reject near-vertical gestures before committing to a horizontal swipe.

  ## Done looks like
  - Diagonal gestures (where `|vy|` is comparable to `|vx|`) do not trigger skin navigation
  - Pure horizontal flicks still work as expected

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — `onMoveShouldSetPanResponder` and `onPanResponderRelease` in `previewPanResponder`

---

#### #98 — Add descriptions to the free milestone skins too
- **Status:** MERGED
- **Created:** 2026-05-29T00:40:08.047Z
- **Updated:** 2026-05-29T01:56:56.720Z
- **Depends on:** #91
- **Proposed from:** #91
- **Category:** incomplete_scope

# Add descriptions to the free milestone skins too

  ## What & Why
  The equip-row now shows descriptions for owned premium skins, but the four free milestone skins (Hornworm, Golden Ladybug, Luna Moth, Emerald Beetle) have no `description` field in `SKIN_UNLOCKS`. The rendering code already supports it — it just needs the data.

  ## Done looks like
  - Each free milestone skin has a short, evocative description in `SKIN_UNLOCKS`
  - Unlocked free skins display their description (italic, muted) in place of the "Unlocked · N pts" line, matching the premium skin treatment

  ## Relevant files
  - `artifacts/mobile/context/GardenContext.tsx` — SKIN_UNLOCKS array (lines 160-168)
  - `artifacts/mobile/app/(tabs)/profile.tsx` — free skin row rendering (lines 628-660)

---

#### #100 — Show a subtle parallax effect on the skin image during a drag
- **Status:** MERGED
- **Created:** 2026-05-29T00:41:46.167Z
- **Updated:** 2026-05-29T02:27:28.351Z
- **Depends on:** #89
- **Proposed from:** #89
- **Category:** next_steps

# Show a subtle parallax effect on the skin image during a drag

  ## What & Why
  The carousel now tracks the finger live with adjacent skins peeking in. A parallax layer — where the skin image moves slightly slower than the card background during a drag — would give depth and a premium feel, similar to iOS app switcher cards.

  ## Done looks like
  - During drag, the skin image within each panel translates at ~60–70% of the drag distance
  - Text labels translate at full speed (normal) so only the image lags behind
  - On snap/release the image eases back to its resting position

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — previewCarouselStrip, previewPanel, previewContentX

---

#### #101 — Let users jump directly to a skin by tapping its dot indicator
- **Status:** MERGED
- **Created:** 2026-05-29T00:41:46.167Z
- **Updated:** 2026-05-29T02:51:13.931Z
- **Depends on:** #89
- **Proposed from:** #89
- **Category:** next_steps

# Let users jump directly to a skin by tapping its dot indicator

  ## What & Why
  The dot indicator at the bottom of the skin preview sheet already renders one dot per skin, but tapping it only works via `jumpToPreviewSkin(i)` (already wired). With many premium skins the row gets long. Adding a scrubbing / long-press gesture on the dot bar would let users jump far across the list quickly without swiping through every skin one-by-one.

  ## Done looks like
  - Long-pressing the dot row activates a scrub mode
  - Dragging across the dot row jumps to the skin under the finger in real time
  - Visual feedback (enlarged active dot, haptic) confirms the current position

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — previewDots, jumpToPreviewSkin

---

#### #102 — Highlight the last-visited free skin so it's easy to spot at a glance
- **Status:** MERGED
- **Created:** 2026-05-29T00:51:10.816Z
- **Updated:** 2026-05-29T03:04:31.315Z
- **Depends on:** #92
- **Proposed from:** #92
- **Category:** next_steps

# Highlight the last-visited free skin so it's easy to spot at a glance

  ## What & Why
  The profile screen now scrolls to the last-tapped free skin on reopen, but the row looks identical to the others — adding a subtle visual highlight (ring, accent background, or "Last viewed" badge) would make it immediately obvious which one was last interacted with, reducing confusion.

  ## Done looks like
  - The last-tapped free skin row has a subtle visual distinction (e.g. soft accent border or background tint) when the profile screen reopens
  - The highlight fades after a second or two so it doesn't feel permanent

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — free skin rows rendered at ~lines 665-715, lastFreeSkinId state added in this task

---

#### #103 — Sync the last-viewed free skin position across devices
- **Status:** MERGED
- **Created:** 2026-05-29T00:51:10.816Z
- **Updated:** 2026-05-29T03:30:08.970Z
- **Depends on:** #92
- **Proposed from:** #92
- **Category:** next_steps

# Sync the last-viewed free skin position across devices

  ## What & Why
  The last-viewed free skin position is currently saved only to local AsyncStorage. If a user switches devices, the scroll position is lost. Storing this value in the cloud garden state (alongside the premium skin index sync) would give full cross-device parity.

  ## Done looks like
  - The last free skin ID is included in cloud sync payloads alongside other garden preferences
  - On login or device switch, the profile screen scrolls to the correct free skin row

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — LAST_FREE_SKIN_ID_KEY and lastFreeSkinId state
  - Garden context / cloud sync logic for existing premium skin index sync

---

#### #104 — Sync the last-visited free skin position across devices
- **Status:** MERGED
- **Created:** 2026-05-29T00:57:30.639Z
- **Updated:** 2026-05-29T03:53:50.332Z
- **Depends on:** #93
- **Proposed from:** #93
- **Category:** next_steps

# Sync the last-visited free skin position across devices

  ## What & Why
  The last-viewed free skin ID (`@quietgrove:last_free_skin_id`) is stored in AsyncStorage but is not included in `ALL_PROFILE_KEYS`, so it doesn't sync to the cloud. Authenticated users who switch devices lose the highlighted free-skin scroll position. This is the free-skin counterpart to the premium skin index sync just completed.

  ## Done looks like
  - `@quietgrove:last_free_skin_id` is added to `ALL_PROFILE_KEYS` in `artifacts/mobile/utils/api.ts`
  - Switching devices restores the last-visited free skin highlight and scroll position in the profile screen

  ## Relevant files
  - `artifacts/mobile/utils/api.ts` — ALL_PROFILE_KEYS
  - `artifacts/mobile/app/(tabs)/profile.tsx` — LAST_FREE_SKIN_ID_KEY, lastFreeSkinId state

---

#### #105 — Show a visual celebration effect when a milestone skin is unlocked
- **Status:** MERGED
- **Created:** 2026-05-29T01:00:58.541Z
- **Updated:** 2026-05-29T04:06:54.641Z
- **Depends on:** #94
- **Proposed from:** #94
- **Category:** next_steps

# Show a visual celebration effect when a milestone skin is unlocked

  ## What & Why
  Right now the unlock is surfaced as a plain toast message. A brief confetti or particle burst at the moment of unlock would make the reward feel genuinely exciting and memorable — matching the emotional weight of earning a rare skin.

  ## Done looks like
  - A short animated celebration (e.g. confetti particles, sparkles, or a radial burst) plays on top of the toast when a free milestone skin is first unlocked
  - The effect is non-blocking and disappears on its own
  - It fires from the same tab-layout layer as the toast

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — where MilestoneSkinsToast lives
  - `artifacts/mobile/context/GardenContext.tsx` — newlyUnlockedSkin state

---

#### #106 — Let users tap the unlock toast to jump straight to the new skin
- **Status:** MERGED
- **Created:** 2026-05-29T01:00:58.541Z
- **Updated:** 2026-05-29T04:19:19.504Z
- **Depends on:** #94
- **Proposed from:** #94
- **Category:** next_steps

# Let users tap the unlock toast to jump straight to the new skin

  ## What & Why
  When the milestone toast fires, the user's natural next step is to go see the skin they just earned. A tappable toast that navigates directly to the skin's slot in the Profile skins screen removes the friction of finding it manually.

  ## Done looks like
  - The MilestoneSkinsToast is tappable (not pointerEvents="none")
  - Tapping it navigates the user to the Profile tab and scrolls / highlights the newly unlocked skin
  - The toast still auto-dismisses if not tapped

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — MilestoneSkinsToast component
  - `artifacts/mobile/app/(tabs)/profile.tsx` — Profile tab with skin carousel

---

#### #107 — Make the snap-in spring feel tighter after a fast flick
- **Status:** MERGED
- **Created:** 2026-05-29T01:13:31.853Z
- **Updated:** 2026-05-29T04:26:39.740Z
- **Depends on:** #95
- **Proposed from:** #95
- **Category:** next_steps

# Make the snap-in spring feel tighter after a fast flick

  ## What & Why
  The slide-out now scales with flick velocity, but the spring-in that follows always uses the same fixed stiffness/damping values. On a very fast flick, a slightly stiffer spring (higher stiffness, lower damping) would reinforce the snappy feel all the way through the transition.

  ## Done looks like
  - The spring-in parameters (stiffness/damping) are scaled based on the same velocity that was passed to the slide-out
  - Fast flicks snap in crisply; slow swipes settle gently as before

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — `jumpToPreviewSkin` function, the `Animated.spring` call at the end of the timing callback

---

#### #108 — Apply velocity-scaled animation to the free skin carousel too
- **Status:** MERGED
- **Created:** 2026-05-29T01:13:31.853Z
- **Updated:** 2026-05-29T05:22:15.392Z
- **Depends on:** #95
- **Proposed from:** #95
- **Category:** next_steps

# Apply velocity-scaled animation to the free skin carousel too

  ## What & Why
  The snappier flick animation was added to the premium skin preview carousel. The free milestone skin carousel (if one exists) likely uses a similar fixed-duration timing. Making it consistent gives users the same native feel everywhere they swipe through skins.

  ## Done looks like
  - Any other skin carousels in the app also scale their slide-out duration with gesture velocity
  - Behavior is consistent: fast flick → shorter duration, slow swipe → full duration

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — search for other `Animated.timing` calls used in swipe/carousel transitions

---

#### #112 — Shared-Device Account Isolation
- **Status:** MERGED
- **Created:** 2026-05-29T01:56:20.339Z
- **Updated:** 2026-05-29T05:40:57.048Z

Vulnerabilities caused by device-local MossLight state surviving sign-out and being reused across later guest or authenticated sessions on the same installation.

Vulnerabilities to fix:

1. [High] Retained local and undo snapshots can be synced into a different account on the same device
  A new user who signs into the app on a shared device can accidentally import the previous user's local garden into their own cloud account. Even when they choose to use their own cloud data, the old device snapshot remains undoable and can still be pushed into the new account afterward.

The root cause is that account transitions do not clear or re-scope the device-local profile cache or the recovery snapshots used by conflict resolution. `performSignOut()` removes only the conflict-preference key and leaves the live profile keys and snapshot keys in place (`artifacts/mobile/app/(tabs)/profile.tsx:160-167`). The retained live data includes the profile snapshot keys enumerated in `ALL_PROFILE_KEYS`, such as `@quietgrove:userPlants`, `@quietgrove:userName`, `@quietgrove:points`, `@quietgrove:friendPrefs`, `@quietgrove:journalEntries`, `@quietgrove:device_id`, and `@quietgrove:greenhouse_code` (`artifacts/mobile/utils/api.ts:7-31`). The recovery features also use global, non-user-scoped keys: `@quietgrove:discarded_snapshot`, `@quietgrove:replaced_by_cloud_snapshot`, and `@quietgrove:pre_merge_snapshot` (`artifacts/mobile/context/GardenContext.tsx:298-303`).

When a different person signs in later on the same device, the sign-in flow treats the retained local plants as current device content. If `userPlants` contains any non-demo data, `handleSSO()` activates the new session, fetches that new account's cloud profile, and then offers conflict actions based on the stale local state (`artifacts/mobile/app/(auth)/sign-in.tsx:165-332`). Choosing "Keep these plants" or "Merge both gardens" immediately calls `forceSyncToCloud()` after the new Clerk session is active, so the previous user's retained local garden is uploaded under the new user's bearer token (`artifacts/mobile/app/(auth)/sign-in.tsx:207-233,274-279,526-535`; `artifacts/mobile/context/GardenContext.tsx:1132-1139`).

The supposedly safer "Use my cloud garden" path is also vulnerable. `restoreFromCloudSnapshot()` first stores the current local garden into `@quietgrove:replaced_by_cloud_snapshot`, then applies the cloud snapshot (`artifacts/mobile/context/GardenContext.tsx:1282-1323`). The tab layout always mounts an undo banner for any signed-in or guest session (`artifacts/mobile/app/(tabs)/_layout.tsx:27-45,130-179,334-377`). If the new user taps Undo, `undoReplaceByCloud()` restores that saved local snapshot and then calls `pushProfile()` with the currently signed-in user's token, overwriting the cloud profile with the previous device user's data (`artifacts/mobile/context/GardenContext.tsx:1325-1362`). The same cross-account problem exists for the other global recovery snapshots used by `discardLocalPlants()` / `undoDiscardLocalPlants()` and `snapshotPreMerge()` / `undoMerge()` (`artifacts/mobile/context/GardenContext.tsx:1142-1275`).

A practical exploit path is: Alice signs out on a shared phone without clearing local state; Bob signs in with his own account; Bob chooses either "Keep these plants" or "Merge both gardens," causing Alice's retained plants and related snapshot data to sync into Bob's server-backed profile. Even if Bob chooses "Use my cloud garden," the app preserves Alice's prior local garden in the undo snapshot, and pressing Undo within the retention window pushes Alice's data into Bob's account anyway. This creates both a confidentiality issue (Alice's data is exposed to Bob during the flow) and a server-side integrity issue (Bob's cloud profile can be overwritten with Alice's retained device state).
  Files: artifacts/mobile/app/(tabs)/profile.tsx, artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/app/(tabs)/_layout.tsx, artifacts/mobile/context/GardenContext.tsx, artifacts/mobile/utils/api.ts

2. [High] Sign-out leaves prior user's garden and journal visible to the next guest on the same device
  Signing out does not clear the previous person's locally cached garden or journal. Anyone who uses the same device afterward can tap “Sign up later” and immediately browse the prior user's plants, notes, and journal history without signing into that account.

The sign-out flow only removes the per-user conflict preference and clears guest mode before redirecting back to the sign-in screen; it does not clear the actual profile cache or journal cache (`artifacts/mobile/app/(tabs)/profile.tsx:160-167`). The shared `GardenProvider` then rehydrates plant and profile state directly from global AsyncStorage keys such as `@quietgrove:userPlants`, `@quietgrove:userName`, `@quietgrove:points`, and related profile keys on every app load regardless of which human is currently using the device (`artifacts/mobile/context/GardenContext.tsx:404-495`). Journal entries are stored separately under a single global key, `@quietgrove:journalEntries`, and are likewise loaded without any account scoping (`artifacts/mobile/hooks/useJournal.ts:12-29`).

After sign-out, the next device user does not need to authenticate to access that retained data. The sign-in screen's “Sign up later” button simply sets `isGuest` and routes into the tab UI (`artifacts/mobile/app/(auth)/sign-in.tsx:361-364`). The tab layout explicitly allows access whenever either `isSignedIn` or `isGuest` is true (`artifacts/mobile/app/(tabs)/_layout.tsx:334-377`). Once inside, multiple screens render the retained state directly, including the profile screen's stats and plant counts (`artifacts/mobile/app/(tabs)/profile.tsx:72-103,276-281`) and the journal screen, which loads and displays all stored entries (`artifacts/mobile/app/(tabs)/journal.tsx:152-156` and `44-77`).

A realistic exploit on a shared family tablet is straightforward: Alice signs out of MossLight, hands the device to Bob, and Bob taps “Sign up later.” Bob is then dropped into the prior local session with Alice's plant collection, nicknames, notes, care history-derived stats, and journal entries still present. This is a cross-user confidentiality failure in the app's stated shared-device threat model, not merely local tampering, because ordinary app navigation exposes one person's cached content to the next person using the same installation.
  Files: artifacts/mobile/app/(tabs)/profile.tsx, artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/app/(tabs)/_layout.tsx, artifacts/mobile/context/GardenContext.tsx, artifacts/mobile/hooks/useJournal.ts, artifacts/mobile/app/(tabs)/journal.tsx

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #114 — Security scan
- **Status:** MERGED
- **Created:** 2026-05-29T01:56:48.411Z
- **Updated:** 2026-05-29T05:49:40.302Z

Run an in-depth scan across the entire project

---

#### #115 — Shared-Device Account Isolation
- **Status:** MERGED
- **Created:** 2026-05-29T02:21:40.611Z
- **Updated:** 2026-05-29T02:38:48.902Z

Vulnerabilities caused by device-local MossLight state surviving sign-out and being reused across later guest or authenticated sessions on the same installation.

Vulnerabilities to fix:

1. [High] Retained local and undo snapshots can be synced into a different account on the same device
  A new user who signs into the app on a shared device can accidentally import the previous user's local garden into their own cloud account. Even when they choose to use their own cloud data, the old device snapshot remains undoable and can still be pushed into the new account afterward.

The root cause is that account transitions do not clear or re-scope the device-local profile cache or the recovery snapshots used by conflict resolution. `performSignOut()` removes only the conflict-preference key and leaves the live profile keys and snapshot keys in place (`artifacts/mobile/app/(tabs)/profile.tsx:160-167`). The retained live data includes the profile snapshot keys enumerated in `ALL_PROFILE_KEYS`, such as `@quietgrove:userPlants`, `@quietgrove:userName`, `@quietgrove:points`, `@quietgrove:friendPrefs`, `@quietgrove:journalEntries`, `@quietgrove:device_id`, and `@quietgrove:greenhouse_code` (`artifacts/mobile/utils/api.ts:7-31`). The recovery features also use global, non-user-scoped keys: `@quietgrove:discarded_snapshot`, `@quietgrove:replaced_by_cloud_snapshot`, and `@quietgrove:pre_merge_snapshot` (`artifacts/mobile/context/GardenContext.tsx:298-303`).

When a different person signs in later on the same device, the sign-in flow treats the retained local plants as current device content. If `userPlants` contains any non-demo data, `handleSSO()` activates the new session, fetches that new account's cloud profile, and then offers conflict actions based on the stale local state (`artifacts/mobile/app/(auth)/sign-in.tsx:165-332`). Choosing "Keep these plants" or "Merge both gardens" immediately calls `forceSyncToCloud()` after the new Clerk session is active, so the previous user's retained local garden is uploaded under the new user's bearer token (`artifacts/mobile/app/(auth)/sign-in.tsx:207-233,274-279,526-535`; `artifacts/mobile/context/GardenContext.tsx:1132-1139`).

The supposedly safer "Use my cloud garden" path is also vulnerable. `restoreFromCloudSnapshot()` first stores the current local garden into `@quietgrove:replaced_by_cloud_snapshot`, then applies the cloud snapshot (`artifacts/mobile/context/GardenContext.tsx:1282-1323`). The tab layout always mounts an undo banner for any signed-in or guest session (`artifacts/mobile/app/(tabs)/_layout.tsx:27-45,130-179,334-377`). If the new user taps Undo, `undoReplaceByCloud()` restores that saved local snapshot and then calls `pushProfile()` with the currently signed-in user's token, overwriting the cloud profile with the previous device user's data (`artifacts/mobile/context/GardenContext.tsx:1325-1362`). The same cross-account problem exists for the other global recovery snapshots used by `discardLocalPlants()` / `undoDiscardLocalPlants()` and `snapshotPreMerge()` / `undoMerge()` (`artifacts/mobile/context/GardenContext.tsx:1142-1275`).

A practical exploit path is: Alice signs out on a shared phone without clearing local state; Bob signs in with his own account; Bob chooses either "Keep these plants" or "Merge both gardens," causing Alice's retained plants and related snapshot data to sync into Bob's server-backed profile. Even if Bob chooses "Use my cloud garden," the app preserves Alice's prior local garden in the undo snapshot, and pressing Undo within the retention window pushes Alice's data into Bob's account anyway. This creates both a confidentiality issue (Alice's data is exposed to Bob during the flow) and a server-side integrity issue (Bob's cloud profile can be overwritten with Alice's retained device state).
  Files: artifacts/mobile/app/(tabs)/profile.tsx, artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/app/(tabs)/_layout.tsx, artifacts/mobile/context/GardenContext.tsx, artifacts/mobile/utils/api.ts

2. [High] Sign-out leaves prior user's garden and journal visible to the next guest on the same device
  Signing out does not clear the previous person's locally cached garden or journal. Anyone who uses the same device afterward can tap “Sign up later” and immediately browse the prior user's plants, notes, and journal history without signing into that account.

The sign-out flow only removes the per-user conflict preference and clears guest mode before redirecting back to the sign-in screen; it does not clear the actual profile cache or journal cache (`artifacts/mobile/app/(tabs)/profile.tsx:160-167`). The shared `GardenProvider` then rehydrates plant and profile state directly from global AsyncStorage keys such as `@quietgrove:userPlants`, `@quietgrove:userName`, `@quietgrove:points`, and related profile keys on every app load regardless of which human is currently using the device (`artifacts/mobile/context/GardenContext.tsx:404-495`). Journal entries are stored separately under a single global key, `@quietgrove:journalEntries`, and are likewise loaded without any account scoping (`artifacts/mobile/hooks/useJournal.ts:12-29`).

After sign-out, the next device user does not need to authenticate to access that retained data. The sign-in screen's “Sign up later” button simply sets `isGuest` and routes into the tab UI (`artifacts/mobile/app/(auth)/sign-in.tsx:361-364`). The tab layout explicitly allows access whenever either `isSignedIn` or `isGuest` is true (`artifacts/mobile/app/(tabs)/_layout.tsx:334-377`). Once inside, multiple screens render the retained state directly, including the profile screen's stats and plant counts (`artifacts/mobile/app/(tabs)/profile.tsx:72-103,276-281`) and the journal screen, which loads and displays all stored entries (`artifacts/mobile/app/(tabs)/journal.tsx:152-156` and `44-77`).

A realistic exploit on a shared family tablet is straightforward: Alice signs out of MossLight, hands the device to Bob, and Bob taps “Sign up later.” Bob is then dropped into the prior local session with Alice's plant collection, nicknames, notes, care history-derived stats, and journal entries still present. This is a cross-user confidentiality failure in the app's stated shared-device threat model, not merely local tampering, because ordinary app navigation exposes one person's cached content to the next person using the same installation.
  Files: artifacts/mobile/app/(tabs)/profile.tsx, artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/app/(tabs)/_layout.tsx, artifacts/mobile/context/GardenContext.tsx, artifacts/mobile/hooks/useJournal.ts, artifacts/mobile/app/(tabs)/journal.tsx

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #117 — Animate dots smoothly while scrubbing across the skin picker
- **Status:** MERGED
- **Created:** 2026-05-29T02:40:57.011Z
- **Updated:** 2026-05-29T06:17:14.999Z
- **Depends on:** #101
- **Proposed from:** #101
- **Category:** next_steps

# Animate dots smoothly while scrubbing across the skin picker

  ## What & Why
  The dot scrub gesture (task #101) jumps to new skins in real time, but the active dot snaps between its wide (18px) and narrow (6px) states instantly. A short spring animation on the dot width and scale would make the scrubbing feel much more polished.

  ## Done looks like
  - The active dot width and the scrub-target scale use Animated.spring transitions instead of hard style switches
  - Transition takes ~120ms so it feels responsive but not jarring
  - No regressions on the existing tap-to-jump behavior

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — previewDots, previewDot styles, dotScrubResponder, isScrubbing state

---

#### #118 — Show a scrub handle tooltip with the skin name while dragging
- **Status:** MERGED
- **Created:** 2026-05-29T02:40:57.011Z
- **Updated:** 2026-05-29T06:22:47.930Z
- **Depends on:** #101
- **Proposed from:** #101
- **Category:** next_steps

# Show a scrub handle tooltip with the skin name while dragging

  ## What & Why
  When users scrub across the dot bar they jump between skins, but the only visual feedback is the carousel animation. A small tooltip above the dot bar showing the current skin's name during scrub would let users target a specific skin without having to read the panel title mid-drag.

  ## Done looks like
  - A floating label appears above the dot row when scrub mode is active (isScrubbing === true)
  - It shows the name of the skin currently under the finger (premiumSkins[previewSkinIndex].label)
  - It fades in when scrubbing starts and fades out on release

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — previewDots, isScrubbing, premiumSkins, previewSkinIndex

---

#### #120 — Persist the highlight trigger across cold app restarts, not just tab switches
- **Status:** MERGED
- **Created:** 2026-05-29T02:53:07.389Z
- **Updated:** 2026-05-29T14:21:47.234Z
- **Depends on:** #102
- **Proposed from:** #102
- **Category:** next_steps

# Persist the highlight trigger across cold app restarts, not just tab switches

  ## What & Why
  The highlight currently fires when lastFreeSkinId changes in state, which covers tab-switch reopens. On a full cold-restart the value is loaded from AsyncStorage but the animation doesn't replay because the effect dependency hasn't changed from its initial load. Users who relaunch the app from scratch won't see the highlight.

  ## Done looks like
  - The highlight animation triggers on the first render after AsyncStorage hydration, whether the user switched tabs or cold-started the app

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — AsyncStorage load at ~line 270, scroll+highlight effect at ~line 285

---

#### #122 — Make sure skin position sync survives going offline and back
- **Status:** MERGED
- **Created:** 2026-05-29T03:30:46.969Z
- **Updated:** 2026-05-29T07:16:32.114Z
- **Depends on:** #104
- **Proposed from:** #104
- **Category:** next_steps

# Make sure skin position sync survives going offline and back

  ## What & Why
  The free and premium skin position keys are now included in the cloud sync payload, but there's no handling for the case where a device is offline when the sync fires. If the push fails silently, the position is lost on the other device. Adding a retry or queued-write mechanism would make the sync bulletproof.

  ## Done looks like
  - A failed profile push is retried automatically (e.g. on next app foreground or network reconnect)
  - Or the sync is optimistically queued and flushed when connectivity is restored
  - No data is silently lost when the device goes offline mid-session

  ## Relevant files
  - `artifacts/mobile/utils/api.ts` — pushProfile, ProfileSyncError
  - `artifacts/mobile/app/(tabs)/profile.tsx` — where sync is triggered

---

#### #123 — Audit all local storage keys to make sure nothing else is missing from cloud sync
- **Status:** MERGED
- **Created:** 2026-05-29T03:30:46.969Z
- **Updated:** 2026-05-29T14:38:03.738Z
- **Depends on:** #104
- **Proposed from:** #104
- **Category:** tech_debt

# Audit all local storage keys to make sure nothing else is missing from cloud sync

  ## What & Why
  Two keys (`last_preview_skin_index` and `last_free_skin_id`) were found missing from `ALL_PROFILE_KEYS` and had to be added individually. There may be other AsyncStorage keys used across the app that are similarly excluded from the sync set, causing silent data loss on device switch.

  ## Done looks like
  - All AsyncStorage keys used anywhere in the mobile app are listed
  - Each is intentionally either included in `ALL_PROFILE_KEYS` or documented as device-local (e.g. device_id)
  - Any newly discovered missing keys are added

  ## Relevant files
  - `artifacts/mobile/utils/api.ts` — ALL_PROFILE_KEYS
  - All screens and utilities that call `AsyncStorage.setItem` or `AsyncStorage.getItem`

---

#### #124 — Let the celebration burst feel different each time it fires
- **Status:** MERGED
- **Created:** 2026-05-29T03:56:54.426Z
- **Updated:** 2026-05-29T14:03:28.892Z
- **Depends on:** #105
- **Proposed from:** #105
- **Category:** next_steps

# Let the celebration burst feel different each time it fires

  ## What & Why
  The current particle burst uses angles and sizes generated once at component mount, so the burst pattern is identical every time a skin is unlocked during the same session. Regenerating particles on each trigger would make every unlock feel fresh and unique.

  ## Done looks like
  - Particle angles, distances, sizes, and delays are regenerated each time `newlyUnlockedSkin` fires
  - The burst still runs smoothly with `useNativeDriver: true`
  - No visible pop or reset between triggers

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — `buildParticles()`, `CelebrationBurst`, `MilestoneSkinsToast`

---

#### #125 — Add a brief screen flash or glow behind the toast when a skin unlocks
- **Status:** MERGED
- **Created:** 2026-05-29T03:56:54.426Z
- **Updated:** 2026-05-29T14:05:43.601Z
- **Depends on:** #105
- **Proposed from:** #105
- **Category:** next_steps

# Add a brief screen flash or glow behind the toast when a skin unlocks

  ## What & Why
  The particle burst adds energy to the unlock moment, but a very short radial glow or full-screen flash (like a camera flash at very low opacity) would make the moment feel even more cinematic. This is a common pattern in games and reward flows.

  ## Done looks like
  - On unlock, a semi-transparent radial gradient or full-screen View flashes in at opacity ~0.15 and fades out in ~400ms
  - It is `pointerEvents="none"` and fully non-blocking
  - It layers behind the toast and particles

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — alongside `CelebrationBurst` and `MilestoneSkinsToast`

---

#### #126 — Make the unlock toast tappable to jump straight to the new skin
- **Status:** MERGED
- **Created:** 2026-05-29T03:56:54.426Z
- **Updated:** 2026-05-29T14:30:40.782Z
- **Depends on:** #105
- **Proposed from:** #105
- **Category:** next_steps

# Make the unlock toast tappable to jump straight to the new skin

  ## What & Why
  After earning a milestone skin users have to navigate manually to the skin picker to equip it. Making the toast itself a tap target that deep-links straight to the unlocked skin would remove that friction and let the reward feel immediately actionable.

  ## Done looks like
  - Tapping the MilestoneSkinsToast navigates to the skin picker screen with the newly unlocked skin highlighted/focused
  - The toast dismisses immediately on tap
  - Works from any tab

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — `MilestoneSkinsToast`
  - Skin picker screen (wherever the skin picker / milestone progress UI lives)

---

#### #127 — Remember which skin unlocked so the highlight still works after a cold app restart
- **Status:** MERGED
- **Created:** 2026-05-29T04:11:11.063Z
- **Updated:** 2026-05-29T15:59:49.111Z
- **Depends on:** #106
- **Proposed from:** #106
- **Category:** incomplete_scope

# Remember which skin unlocked so the highlight still works after a cold app restart

  ## What & Why
  The current tappable toast uses an in-memory context value (`pendingHighlightSkinId`) to pass the skin ID to the Profile screen. If the app is cold-started or the tab layout unmounts between the toast firing and the user tapping, the highlight is lost. Persisting the pending skin ID to AsyncStorage would make the highlight survive process kills.

  ## Done looks like
  - `pendingHighlightSkinId` is written to AsyncStorage when set and cleared when consumed
  - On app launch, GardenContext reads the persisted value and restores it so Profile can still scroll to the right skin
  - The highlight fires at most once per unlock (no duplicate flashes on subsequent opens)

  ## Relevant files
  - `artifacts/mobile/context/GardenContext.tsx` — add AsyncStorage read/write around `pendingHighlightSkinId`
  - `artifacts/mobile/app/(tabs)/profile.tsx` — already consumes `pendingHighlightSkinId` via context

---

#### #128 — Show a small chevron or arrow icon on the unlock toast so it's obviously tappable
- **Status:** MERGED
- **Created:** 2026-05-29T04:11:11.063Z
- **Updated:** 2026-05-29T16:14:58.104Z
- **Depends on:** #106
- **Proposed from:** #106
- **Category:** next_steps

# Show a small chevron or arrow icon on the unlock toast so it's obviously tappable

  ## What & Why
  The toast currently uses the text "Tap to view →" to hint at tappability, but a small icon (e.g. a chevron-forward or external-link symbol) alongside the text would make the affordance immediately obvious at a glance, matching conventions users expect from tappable toasts in other apps.

  ## Done looks like
  - The MilestoneSkinsToast shows an `AppIcon` (chevron-forward or similar) at the trailing edge
  - The icon animates in with the toast (no separate animation required)
  - The text label is shortened slightly to accommodate the icon without wrapping

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — MilestoneSkinsToast component and skinsToast/skinsToastTouchable styles

---

#### #132 — Shared-Device Account Isolation
- **Status:** MERGED
- **Created:** 2026-05-29T05:44:16.119Z
- **Updated:** 2026-05-29T05:49:42.169Z

Vulnerabilities caused by device-local MossLight state surviving sign-out and being reused across later guest or authenticated sessions on the same installation.

Vulnerabilities to fix:

1. [High] Retained local and undo snapshots can be synced into a different account on the same device
  A new user who signs into the app on a shared device can accidentally import the previous user's local garden into their own cloud account. Even when they choose to use their own cloud data, the old device snapshot remains undoable and can still be pushed into the new account afterward.

The root cause is that account transitions do not clear or re-scope the device-local profile cache or the recovery snapshots used by conflict resolution. `performSignOut()` removes only the conflict-preference key and leaves the live profile keys and snapshot keys in place (`artifacts/mobile/app/(tabs)/profile.tsx:160-167`). The retained live data includes the profile snapshot keys enumerated in `ALL_PROFILE_KEYS`, such as `@quietgrove:userPlants`, `@quietgrove:userName`, `@quietgrove:points`, `@quietgrove:friendPrefs`, `@quietgrove:journalEntries`, `@quietgrove:device_id`, and `@quietgrove:greenhouse_code` (`artifacts/mobile/utils/api.ts:7-31`). The recovery features also use global, non-user-scoped keys: `@quietgrove:discarded_snapshot`, `@quietgrove:replaced_by_cloud_snapshot`, and `@quietgrove:pre_merge_snapshot` (`artifacts/mobile/context/GardenContext.tsx:298-303`).

When a different person signs in later on the same device, the sign-in flow treats the retained local plants as current device content. If `userPlants` contains any non-demo data, `handleSSO()` activates the new session, fetches that new account's cloud profile, and then offers conflict actions based on the stale local state (`artifacts/mobile/app/(auth)/sign-in.tsx:165-332`). Choosing "Keep these plants" or "Merge both gardens" immediately calls `forceSyncToCloud()` after the new Clerk session is active, so the previous user's retained local garden is uploaded under the new user's bearer token (`artifacts/mobile/app/(auth)/sign-in.tsx:207-233,274-279,526-535`; `artifacts/mobile/context/GardenContext.tsx:1132-1139`).

The supposedly safer "Use my cloud garden" path is also vulnerable. `restoreFromCloudSnapshot()` first stores the current local garden into `@quietgrove:replaced_by_cloud_snapshot`, then applies the cloud snapshot (`artifacts/mobile/context/GardenContext.tsx:1282-1323`). The tab layout always mounts an undo banner for any signed-in or guest session (`artifacts/mobile/app/(tabs)/_layout.tsx:27-45,130-179,334-377`). If the new user taps Undo, `undoReplaceByCloud()` restores that saved local snapshot and then calls `pushProfile()` with the currently signed-in user's token, overwriting the cloud profile with the previous device user's data (`artifacts/mobile/context/GardenContext.tsx:1325-1362`). The same cross-account problem exists for the other global recovery snapshots used by `discardLocalPlants()` / `undoDiscardLocalPlants()` and `snapshotPreMerge()` / `undoMerge()` (`artifacts/mobile/context/GardenContext.tsx:1142-1275`).

A practical exploit path is: Alice signs out on a shared phone without clearing local state; Bob signs in with his own account; Bob chooses either "Keep these plants" or "Merge both gardens," causing Alice's retained plants and related snapshot data to sync into Bob's server-backed profile. Even if Bob chooses "Use my cloud garden," the app preserves Alice's prior local garden in the undo snapshot, and pressing Undo within the retention window pushes Alice's data into Bob's account anyway. This creates both a confidentiality issue (Alice's data is exposed to Bob during the flow) and a server-side integrity issue (Bob's cloud profile can be overwritten with Alice's retained device state).
  Files: artifacts/mobile/app/(tabs)/profile.tsx, artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/app/(tabs)/_layout.tsx, artifacts/mobile/context/GardenContext.tsx, artifacts/mobile/utils/api.ts

2. [High] Sign-out leaves prior user's garden and journal visible to the next guest on the same device
  Signing out does not clear the previous person's locally cached garden or journal. Anyone who uses the same device afterward can tap “Sign up later” and immediately browse the prior user's plants, notes, and journal history without signing into that account.

The sign-out flow only removes the per-user conflict preference and clears guest mode before redirecting back to the sign-in screen; it does not clear the actual profile cache or journal cache (`artifacts/mobile/app/(tabs)/profile.tsx:160-167`). The shared `GardenProvider` then rehydrates plant and profile state directly from global AsyncStorage keys such as `@quietgrove:userPlants`, `@quietgrove:userName`, `@quietgrove:points`, and related profile keys on every app load regardless of which human is currently using the device (`artifacts/mobile/context/GardenContext.tsx:404-495`). Journal entries are stored separately under a single global key, `@quietgrove:journalEntries`, and are likewise loaded without any account scoping (`artifacts/mobile/hooks/useJournal.ts:12-29`).

After sign-out, the next device user does not need to authenticate to access that retained data. The sign-in screen's “Sign up later” button simply sets `isGuest` and routes into the tab UI (`artifacts/mobile/app/(auth)/sign-in.tsx:361-364`). The tab layout explicitly allows access whenever either `isSignedIn` or `isGuest` is true (`artifacts/mobile/app/(tabs)/_layout.tsx:334-377`). Once inside, multiple screens render the retained state directly, including the profile screen's stats and plant counts (`artifacts/mobile/app/(tabs)/profile.tsx:72-103,276-281`) and the journal screen, which loads and displays all stored entries (`artifacts/mobile/app/(tabs)/journal.tsx:152-156` and `44-77`).

A realistic exploit on a shared family tablet is straightforward: Alice signs out of MossLight, hands the device to Bob, and Bob taps “Sign up later.” Bob is then dropped into the prior local session with Alice's plant collection, nicknames, notes, care history-derived stats, and journal entries still present. This is a cross-user confidentiality failure in the app's stated shared-device threat model, not merely local tampering, because ordinary app navigation exposes one person's cached content to the next person using the same installation.
  Files: artifacts/mobile/app/(tabs)/profile.tsx, artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/app/(tabs)/_layout.tsx, artifacts/mobile/context/GardenContext.tsx, artifacts/mobile/hooks/useJournal.ts, artifacts/mobile/app/(tabs)/journal.tsx

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #133 — Shared-Device Account Isolation
- **Status:** MERGED
- **Created:** 2026-05-29T05:49:27.582Z
- **Updated:** 2026-05-29T16:40:11.931Z

Mobile vulnerabilities where retained AsyncStorage state can be exposed to or synced into the wrong user account during guest, sign-in, sign-out, or recovery flows.

Vulnerabilities to fix:

1. [Medium] Guest 'Start fresh' still imports retained journal and profile state into the next account
  When someone signs into a new account from guest mode and chooses “Start fresh,” the app only deletes plant records. The previous guest’s journal entries, name, reading state, and other personal data stay on the device, become visible in the signed-in session, and are then uploaded into the new account’s cloud backup.

The bug is in the fresh-account guest sign-in path. In `artifacts/mobile/app/(auth)/sign-in.tsx`, `showBringGardenPrompt()` is used when a guest has local content and the destination account has no cloud data. If the user chooses "Start fresh," the flow calls `discardLocalPlants()` and then resumes sync (`artifacts/mobile/app/(auth)/sign-in.tsx:131-155,387-388`). `discardLocalPlants()` only snapshots and removes plant-related state: `userPlants`, `totalWaterings`, `totalFertilizings`, and `points` (`artifacts/mobile/context/GardenContext.tsx:1177-1205`). It does not clear other user-scoped data such as `@quietgrove:journalEntries`, `@quietgrove:lastCheckIn`, `@quietgrove:userName`, `@quietgrove:learnRead`, `@quietgrove:speciesRead`, `@quietgrove:device_id`, or `@quietgrove:greenhouse_code`.

Those retained keys are still treated as part of the synced profile. `ALL_PROFILE_KEYS` in `artifacts/mobile/utils/api.ts` explicitly includes `@quietgrove:journalEntries`, `@quietgrove:lastCheckIn`, `@quietgrove:userName`, `@quietgrove:learnRead`, `@quietgrove:speciesRead`, `@quietgrove:device_id`, and `@quietgrove:greenhouse_code` (`artifacts/mobile/utils/api.ts:7-33`). The background sync effect later builds a full snapshot from those keys and pushes it to the authenticated user’s profile once the app stops treating the session as guest mode (`artifacts/mobile/context/GardenContext.tsx:681-703`; `artifacts/mobile/app/_layout.tsx:141-146`). As a result, the prior guest’s leftover journal and profile data are silently imported into the newly signed-in account even though the user selected a flow labeled as starting fresh.

The disclosure is also visible locally before the upload. Journal entries are stored globally under `@quietgrove:journalEntries` and loaded without account scoping by `useJournal()` (`artifacts/mobile/hooks/useJournal.ts:12-29`). Daily check-in state is likewise global under `@quietgrove:lastCheckIn` (`artifacts/mobile/hooks/useDailyPrompt.ts:5,20-30`). After the new user signs in and opens the app’s journal-related screens, they can see the previous guest’s retained entries and activity state because the "Start fresh" path never cleared those keys.

A realistic exploit path is: Alice uses the app as a guest on a shared tablet and writes journal entries; Bob later signs into his brand-new account on the same device and chooses "Start fresh" because he does not want Alice’s garden. Bob still inherits Alice’s journal and related retained state locally, and within one sync cycle that stale data is uploaded into Bob’s cloud profile. This breaks both shared-device confidentiality and account-isolation guarantees, even though the user chose the supposedly safe fresh-start option.
  Files: artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/app/_layout.tsx, artifacts/mobile/context/GardenContext.tsx, artifacts/mobile/utils/api.ts, artifacts/mobile/hooks/useJournal.ts, artifacts/mobile/hooks/useDailyPrompt.ts

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #147 — Security scan
- **Status:** MERGED
- **Created:** 2026-05-29T19:50:40.327Z
- **Updated:** 2026-05-29T21:02:58.460Z

Run an in-depth scan across the entire project

---

#### #148 — Authentication Proxy Abuse
- **Status:** MERGED
- **Created:** 2026-05-30T00:39:19.870Z
- **Updated:** 2026-05-30T00:43:05.335Z

Vulnerabilities in the public Clerk proxy and related auth-boundary handling that weaken authentication abuse protections.

Vulnerabilities to fix:

1. [Medium] Public Clerk proxy trusts client-supplied X-Forwarded-For
  The public authentication proxy lets anyone on the internet choose the IP address that gets forwarded to Clerk. That weakens Clerk's per-IP sign-in and sign-up protections, making brute-force and abuse throttles much easier to evade.

`artifacts/api-server/src/app.ts:33-36` mounts `clerkProxyMiddleware()` on the public `/api/__clerk` path before any authentication checks, so unauthenticated internet clients can directly reach the proxy. Inside `artifacts/api-server/src/middlewares/clerkProxyMiddleware.ts:80-87`, the middleware reads `req.headers["x-forwarded-for"]`, takes the leftmost value, and forwards it upstream as `X-Forwarded-For` without verifying that the header came from a trusted reverse proxy. Because `X-Forwarded-For` is just another request header at this boundary, an attacker can send requests such as `X-Forwarded-For: 1.2.3.4` and make Clerk see an arbitrary client IP instead of the attacker's real source address.

This matters because Clerk's Frontend API rate limits are keyed by client IP for sensitive authentication actions such as creating sign-ins, creating sign-ups, and attempting verification factors. By rotating a fake `X-Forwarded-For` value on each request, an attacker can spread traffic across unlimited spoofed IPs while still using the same real connection. That undermines Clerk's built-in throttling and abuse detection on the application's public auth surface, making credential-stuffing, signup spam, and repeated authentication attempts materially easier than intended.

A realistic exploit path is straightforward: an attacker scripts requests to `/api/__clerk/...` and changes `X-Forwarded-For` on every attempt. The application forwards those spoofed IPs to Clerk because it prefers the untrusted header over `req.socket.remoteAddress`. The attacker can then exceed the per-IP request budgets that Clerk would normally apply to a single source, reducing the effectiveness of the app's primary external authentication control.
  Files: artifacts/api-server/src/app.ts, artifacts/api-server/src/middlewares/clerkProxyMiddleware.ts

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #149 — Release Credentials
- **Status:** MERGED
- **Created:** 2026-05-30T00:39:19.870Z
- **Updated:** 2026-05-30T01:11:18.690Z

Vulnerabilities that expose build, signing, or release-pipeline credentials and could let an attacker tamper with production mobile releases.

Vulnerabilities to fix:

1. [High] Hardcoded Expo personal access token enables mobile release pipeline abuse
  A real Expo account credential is checked into source control in the Windows build script. Anyone who can read the repository can reuse that token to act as the app owner in Expo's build system, which can let them tamper with mobile releases, break signing automation, or abuse the team's build account.

`artifacts/mobile/build.bat:31` hardcodes `EXPO_TOKEN=vs7T1bXm4FxVCAyQTzLzo8H-DX3aF2NVXaJNFtCb` and immediately invokes `pnpm exec eas build --platform ios --profile production --auto-submit --non-interactive`. This is not a harmless identifier: Expo's programmatic-access documentation states that anyone with an `EXPO_TOKEN` can perform actions against the account and should treat the token with the same care as a password, and the CI documentation states that once `EXPO_TOKEN` is set you can run EAS CLI commands authenticated as the account owner.

The rest of the repository shows that this token is powerful enough to affect the production delivery pipeline. `scripts/clear-eas-profile.mjs:8-18` uses `EXPO_TOKEN` as a bearer token against `https://api.expo.dev/graphql`, then deletes provisioning profiles for `@mtwalcott/mosslight`. `scripts/reset-pp.mjs:3-10` does the same. `scripts/set-team-type.mjs:4-10` uses the same token to query and update Apple team metadata in Expo. In other words, the leaked token is already wired for privileged, state-changing operations, not just read-only telemetry.

An attacker with repository read access can take this token and authenticate to Expo as the `mtwalcott` account, trigger or interfere with iOS build jobs, delete or alter Expo-managed signing state, and potentially drive malicious or disruptive release actions through the existing EAS configuration. Even if some App Store operations also require the separate ASC private key, this token alone is sufficient for meaningful integrity and availability impact on the mobile release pipeline. The credential should be treated as compromised, revoked in Expo, replaced with a new token stored only in the secrets manager/CI environment, and removed from source history.
  Files: artifacts/mobile/build.bat, scripts/clear-eas-profile.mjs, scripts/reset-pp.mjs, scripts/set-team-type.mjs

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #150 — Shared-Device Account Isolation
- **Status:** MERGED
- **Created:** 2026-05-30T00:39:19.870Z
- **Updated:** 2026-05-30T01:27:13.321Z

Vulnerabilities where retained local state, guest mode, or undo flows let one device user access or sync another person's data.

Vulnerabilities to fix:

1. [Medium] Guest mode persists the previous person's garden and journal across app relaunches
  If one person uses the app in guest mode on a shared phone or tablet and simply closes it, the next person who opens the app is dropped back into that same guest session and can read or modify the previous person's plants and journal entries.

Guest mode is stored as a durable device flag, not as a one-time session. `handleSkip()` sets guest mode and routes directly into the tabs experience (`artifacts/mobile/app/(auth)/sign-in.tsx:421-431`), while `setIsGuest()` persists `@quietgrove:guest_mode` in AsyncStorage (`artifacts/mobile/context/GardenContext.tsx:432-439`). On startup, `GardenProvider` rehydrates the prior local state from AsyncStorage, including `userPlants`, points, read state, and the guest flag (`artifacts/mobile/context/GardenContext.tsx:441-515`). The tab router then grants full access whenever either `isSignedIn` or `isGuest` is true, and only redirects to the sign-in flow when both are false (`artifacts/mobile/app/(tabs)/_layout.tsx:541-571`).

Journal and daily check-in data are also stored globally on the device with no user scoping. `useJournal()` always loads `@quietgrove:journalEntries` from AsyncStorage (`artifacts/mobile/hooks/useJournal.ts:12-29`), and `useDailyPrompt()` does the same for `@quietgrove:lastCheckIn` (`artifacts/mobile/hooks/useDailyPrompt.ts:5-30`). As a result, reopening the app in guest mode exposes not just a plant count banner but the full retained local garden and journal state of the previous human user.

A realistic exploit path is simple: Alice uses the app as a guest on a family tablet, records journal entries, and closes the app without explicitly using the sign-out action in Profile. Bob opens the app later and is taken straight into Alice's guest session because `@quietgrove:guest_mode` is still set. Bob can browse Alice's plants and journal, modify or delete them, and later attach that retained state to another account through the guest sign-in flows. This breaks the shared-device isolation guarantees in the threat model even though no server compromise is required.
  Files: artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/app/(tabs)/_layout.tsx, artifacts/mobile/context/GardenContext.tsx, artifacts/mobile/hooks/useJournal.ts, artifacts/mobile/hooks/useDailyPrompt.ts

2. [Medium] Choosing 'Use my cloud garden' still leaves an undo path to restore prior guest data into the signed-in account
  When a new person signs into an existing account on a shared device and chooses to keep their cloud garden, the app still keeps a one-tap undo copy of the previous guest session. That lets the signed-in user restore the earlier local data and sync it into the account they just signed into.

The guest conflict flow pauses sync and fetches the incoming account's cloud profile when local guest plants already exist (`artifacts/mobile/app/(auth)/sign-in.tsx:224-248`). If the account has cloud data and the user chooses `use_cloud`, the code calls `restoreFromCloudSnapshot(cloudData!.snapshot, cloudData!.code)` in both the saved-preference fast path and the explicit confirmation path (`artifacts/mobile/app/(auth)/sign-in.tsx:277-285` and `artifacts/mobile/app/(auth)/sign-in.tsx:367-375`). Unlike the separate stale-authenticated-user path earlier in the same file, these guest branches never call `clearReplacedByCloudSnapshot()` after the restore.

That omission matters because `restoreFromCloudSnapshot()` always saves the current local state into `@quietgrove:replaced_by_cloud_snapshot` before applying the cloud snapshot (`artifacts/mobile/context/GardenContext.tsx:1384-1399`). The tab layout shows an undo banner whenever `replacedByCloudSnapshotTs` is present (`artifacts/mobile/app/(tabs)/_layout.tsx:284-307`). Pressing Undo invokes `undoReplaceByCloud()`, which restores the saved local plants, water/fertilizer counts, and points, then rebuilds the full profile snapshot and pushes it to the authenticated cloud profile with `pushProfile()` (`artifacts/mobile/context/GardenContext.tsx:1427-1461`; `artifacts/mobile/utils/api.ts:43-95`).

A realistic exploit path is: Alice uses the device as a guest and leaves plants in local storage. Bob later signs into his own existing MossLight account on that device and chooses “Use my cloud garden” because he does not want Alice's local garden. The app restores Bob's cloud data but immediately offers an Undo action backed by Alice's saved guest snapshot. If Bob taps Undo, Alice's retained local garden is restored inside Bob's authenticated session and then synced to Bob's account. Because the `use_cloud` preference is also stored per account, the same restore-plus-undo path can reappear silently on later shared-device sign-ins for that account.
  Files: artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/app/(tabs)/_layout.tsx, artifacts/mobile/context/GardenContext.tsx, artifacts/mobile/utils/api.ts

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #151 — Security scan
- **Status:** MERGED
- **Created:** 2026-05-30T04:24:32.246Z
- **Updated:** 2026-05-30T11:14:10.970Z

Run an in-depth scan across the entire project

---

#### #152 — Release Pipeline Credentials
- **Status:** MERGED
- **Created:** 2026-05-30T10:56:00.794Z
- **Updated:** 2026-05-30T11:34:01.240Z

Compromise of build and submission credentials that can affect production mobile release integrity.

Vulnerabilities to fix:

1. [High] App Store Connect API private key committed to repository
  The repository contains a live Apple App Store Connect private key used by the release pipeline. Anyone who can read the repo can take that key and authenticate to the app's Apple release account, which can let them tamper with TestFlight or App Store release operations.

A real App Store Connect API key is committed at `attached_assets/AuthKey_D58MSPKL8X_1779993185321.p8` (`-----BEGIN PRIVATE KEY-----` at lines 1-6). The repository also publishes the matching key identifiers and target app details needed to use it: `.replit` hardcodes `EXPO_ASC_KEY_ID=D58MSPKL8X` and `EXPO_ASC_ISSUER_ID=56ef3e89-0464-4cb0-9893-edf723744f24` in the production build and submit workflows (lines 36 and 44), and `artifacts/mobile/eas.json` exposes the Apple account email, App Store Connect app ID, and Apple team ID (lines 41-45). A committed Expo credentials PDF further confirms the production bundle identifier, team, distribution certificate serial, and provisioning profile UUID in `attached_assets/iOS_Credentials_–_@mtwalcott_mosslight_—_Expo_1779988612332.pdf`.

This is exploitable by any repository reader: they can download the `.p8` file, mint a valid App Store Connect JWT with the exposed key ID and issuer ID, and call Apple's API as the release automation identity. The exact blast radius depends on the role assigned to that key in App Store Connect, but the workflow proves it is at least powerful enough to drive automated production submission. That can enable unauthorized viewing or modification of TestFlight/App Store release state, release metadata changes, submission abuse, or disruption of the mobile delivery pipeline. Because the credential is committed to source rather than held only in a secret store, it should be treated as compromised and rotated.
  Files: attached_assets/AuthKey_D58MSPKL8X_1779993185321.p8, .replit, artifacts/mobile/eas.json, attached_assets/iOS_Credentials_–_@mtwalcott_mosslight_—_Expo_1779988612332.pdf

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #153 — Auth and Session Boundaries
- **Status:** MERGED
- **Created:** 2026-05-30T10:56:00.794Z
- **Updated:** 2026-05-30T12:21:58.483Z

Vulnerabilities in authentication token validation, public auth proxy handling, and browser session-token exposure.

Vulnerabilities to fix:

1. [Medium] Authenticated API accepts Clerk tokens from untrusted origins
  The profile API accepts any valid Clerk session token for this Clerk instance, even if that token was minted from a different website. That means a phishing page or rogue frontend using MossLight's public Clerk configuration could obtain a victim's session token and then read or overwrite that victim's synced garden data through the production API.

`requireAuth()` in `artifacts/api-server/src/lib/auth.ts` verifies bearer tokens with `verifyToken(token, { secretKey })` only (line 26) and does not supply Clerk's `authorizedParties` allowlist. Clerk documents this option as the control that binds a session token's `azp` claim to the expected frontend origin and specifically recommends it to prevent tokens issued for other origins from being accepted. As implemented, the server trusts any token signed by the same Clerk tenant, regardless of which origin obtained it.

That trust mistake matters because every sensitive sync endpoint hangs directly off this middleware. `artifacts/api-server/src/routes/profile.ts` uses `requireAuth` for `GET /profile`, `POST /profile/code`, `GET /profile/:code`, and `PUT /profile/:code` (lines 44, 76, 121, and 152). Once an attacker has a victim's valid MossLight Clerk token, they can call `GET /api/profile` to retrieve the victim's full synced snapshot and greenhouse code, then call `PUT /api/profile/:code` to replace the stored snapshot with attacker-controlled JSON. CORS does not meaningfully mitigate this because a malicious site can simply exfiltrate the token to its own backend and make the API calls server-to-server.

A practical attack path is: (1) the attacker stands up a fake MossLight sign-in page or other frontend wired to the same public Clerk publishable key, (2) the victim authenticates there, (3) the attacker captures the returned session token, and (4) the attacker reuses that token against `https://MossLight.replit.app/api/profile` to read and tamper with the victim's cloud data. The fix is to bind token verification to MossLight's real allowed origins with `authorizedParties` (and, if a custom audience is ever introduced, validate `audience` as well).
  Files: artifacts/api-server/src/lib/auth.ts, artifacts/api-server/src/routes/profile.ts

2. [Medium] Clerk proxy collapses all users into one shared IP bucket
  The public authentication proxy sends Clerk the deployment proxy's IP address instead of the real visitor's IP address. That means one attacker can consume the shared rate-limit bucket for every user and temporarily block sign-in or sign-up traffic across the whole deployment.

`artifacts/api-server/src/app.ts` mounts `clerkProxyMiddleware()` on the public unauthenticated path `/api/__clerk/**` before any authentication or request filtering. Inside `artifacts/api-server/src/middlewares/clerkProxyMiddleware.ts`, the proxy handler rebuilds Clerk's required forwarding headers and sets `X-Forwarded-For` from `req.socket.remoteAddress` (lines 72-83). In a production Replit deployment, `req.socket.remoteAddress` is the address of the reverse proxy/load balancer connected to the Node process, not the original internet client. The application also does not configure Express proxy trust anywhere in `app.ts`, so there is no trusted-proxy path that recovers the real client IP before forwarding.

Clerk's proxy documentation requires `X-Forwarded-For` to contain the original client IP for Frontend API proxying, because Clerk applies abuse controls and rate limits per client IP. By overwriting that header with the shared ingress IP, the app makes every user's `/api/__clerk/**` traffic appear to come from the same address. An unauthenticated attacker can repeatedly hit Clerk-backed login, verification, or session-establishment endpoints through `/api/__clerk/**` until Clerk rate-limits that shared IP or increases its abuse score. Once that happens, legitimate users behind the same deployment are throttled as well and may receive `429` responses or additional anti-abuse challenges during authentication. This is a production-reachable denial-of-service weakness on the public auth surface.
  Files: artifacts/api-server/src/app.ts, artifacts/api-server/src/middlewares/clerkProxyMiddleware.ts

3. [High] Web auth tokens are exposed to same-origin third-party script execution
  The web app stores Clerk session tokens in browser localStorage, and the same deployment origin also serves a page that loads JavaScript directly from `unpkg.com`. If that third-party script is ever compromised, it can read the stored tokens and take over logged-in web accounts.

In `artifacts/mobile/app/_layout.tsx`, the web-specific Clerk token cache persists bearer tokens with `localStorage.setItem(...)` and reads them back with `localStorage.getItem(...)` (lines 45-66). Those tokens are therefore available to any JavaScript that executes in pages on the `https://MossLight.replit.app` origin. Separately, `artifacts/mobile/server/serve.js` serves `landing-page.html` on `/` for ordinary browser requests (lines 118-126), and that page dynamically injects `<script src="https://unpkg.com/qr-code-styling@1.6.0/lib/qr-code-styling.js">` without self-hosting or an integrity pin in `artifacts/mobile/server/templates/landing-page.html` (lines 467-474).

Because third-party scripts execute with the embedding page's origin privileges, the `unpkg.com` script runs with full access to MossLight's origin storage. Any compromise of that CDN asset, malicious package update, or dependency takeover would let the injected script read Clerk tokens from `localStorage` and exfiltrate them. An attacker could then replay the stolen bearer token against authenticated endpoints such as `/api/profile` to read or overwrite the victim's synced garden snapshot. This is a concrete account-takeover path for web users because the application itself combines two unsafe choices on the same origin: bearer-token storage in `localStorage` and execution of remote third-party JavaScript. The safe remediation is to avoid exposing auth tokens to page JavaScript on web (for example by using HttpOnly cookies or Clerk's server-managed browser flow) and/or remove same-origin third-party script execution by self-hosting the QR code library and enforcing a restrictive CSP/SRI policy.
  Files: artifacts/mobile/app/_layout.tsx, artifacts/mobile/server/templates/landing-page.html, artifacts/mobile/server/serve.js

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #154 — Shared-Device Sync Isolation
- **Status:** MERGED
- **Created:** 2026-05-30T10:56:00.844Z
- **Updated:** 2026-05-30T12:39:56.847Z

Cross-account data isolation failures in the sign-in, restore, and background sync flows for reused devices.

Vulnerabilities to fix:

1. [Medium] Cloud restore leaves prior user's in-memory state visible to the next account
  After one person signs in on a reused device, the app can continue showing pieces of the previous person's local data even though it has loaded the new account's cloud profile. That stale data is not just cosmetic: some of it can be written back into the new user's account the next time they interact with those features.

The root cause is `restoreFromCloudSnapshot()` in `artifacts/mobile/context/GardenContext.tsx`. The function calls `applySnapshot(snap)`, which correctly updates AsyncStorage for every key in the fetched cloud snapshot, but it only refreshes a subset of the corresponding React state variables in memory. It conditionally updates plants, name, tier, counts, critter prefs, points, friend prefs, animation flags, active skins, purchased premium skins, unlock state, and beetle-choice state, but it does not reset or rehydrate several other user-scoped values that are also part of the synced profile, including:

- `customLocations`
- `notificationsEnabled`
- `notificationHour`
- `learnRead`
- `speciesRead`

These values remain whatever the previous device user had in memory before the restore. The shared-device sign-in flow in `artifacts/mobile/app/(auth)/sign-in.tsx` relies on `restoreFromCloudSnapshot()` both when replacing stale local state from a prior authenticated user and when a guest chooses to use an existing cloud garden, so the leak is reachable during real account transitions.

The stale values are then exposed by normal UI components. For example:

- `artifacts/mobile/app/(tabs)/index.tsx` displays `customLocations` in the location manager.
- `artifacts/mobile/app/(tabs)/discover.tsx` uses `learnRead` and `speciesRead` to show reading progress and which topics/species were already opened.
- `artifacts/mobile/app/settings.tsx` uses `notificationsEnabled` and `notificationHour` to render reminder settings.

This is not limited to read-only disclosure. Several setters merge or extend the existing in-memory state before writing it back to storage. For example, `addCustomLocation()` appends to the current `customLocations` array, and `markLearnRead()` / `markSpeciesRead()` add to the current Sets before persisting them. As a result, if user A's custom locations or reading history remain in memory when user B signs in, user B's next interaction can save the union of A and B's data into B's local snapshot and later sync it to B's cloud profile.

A practical exploit is: user A uses a shared device and creates custom room names or accumulates Herbarium read markers; user B then signs into an account with existing plants, causing the app to take the cloud-restore path; because those fields are never reset in memory, B immediately sees A's locations/progress and any subsequent add/read action persists A's stale state into B's account. This creates both cross-account information disclosure and cross-account state pollution within the application's stated shared-device threat model.
  Files: artifacts/mobile/context/GardenContext.tsx, artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/app/(tabs)/index.tsx, artifacts/mobile/app/(tabs)/discover.tsx, artifacts/mobile/app/settings.tsx

2. [Medium] Account switch logic wipes cloud profiles that only contain non-plant data
  When someone signs into an existing account on a shared device, the app decides whether that account has cloud data by looking only for "real" plants. If the account's synced data consists of journal entries, read-state, settings, greenhouse code, or other non-plant profile data, the app treats it as empty and can overwrite that user's cloud backup with a blank profile.

In `artifacts/mobile/app/(auth)/sign-in.tsx`, the shared-device recovery path for a stale prior authenticated session fetches the incoming user's cloud snapshot and then computes `incomingHasData` solely from `@quietgrove:userPlants`:

- `incomingPlantsRaw = incomingProfile?.snapshot?.["@quietgrove:userPlants"]`
- `incomingPlants = incomingPlantsRaw ? JSON.parse(incomingPlantsRaw) : []`
- `incomingHasData = incomingPlants.some(isRealPlant)`
- if false, the code executes `await clearLocalState()` instead of restoring the fetched snapshot.

That heuristic ignores the fact that `artifacts/mobile/utils/api.ts` defines `ALL_PROFILE_KEYS` to include many other synced fields such as `@quietgrove:journalEntries`, `@quietgrove:learnRead`, `@quietgrove:speciesRead`, `@quietgrove:theme`, `@quietgrove:greenhouse_code`, notification settings, skin state, and custom locations. An account can therefore have legitimate cloud data while still failing the `incomingHasData` check.

Because sync is only paused temporarily, once `clearLocalState()` finishes and `resumeSync()` runs, the normal background sync path in `GardenContext` will build a snapshot from the now-empty local storage and `PUT` it back to `/profile/:code`. In practice, this means a second person signing into their own account on a reused device can have their existing non-plant cloud state silently erased simply because the prior device user left authenticated local state behind and the victim's account does not currently contain any non-demo plants.

A realistic exploitation scenario is: user A force-closes the app while signed in on a shared device; user B later signs into an established MossLight account that contains journal/history/preferences but no current plants; the sign-in flow misclassifies B's account as empty, clears local state, resumes sync, and eventually overwrites B's cloud snapshot with an empty one. This is a cross-account integrity failure within the shared-device threat model.
  Files: artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/utils/api.ts

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #155 — Security scan
- **Status:** MERGED
- **Created:** 2026-05-30T11:27:26.465Z
- **Updated:** 2026-05-30T12:56:31.536Z

Run an in-depth scan across the entire project

---

#### #156 — Authentication and Proxy Trust Issues
- **Status:** MERGED
- **Created:** 2026-05-30T12:50:38.062Z
- **Updated:** 2026-05-30T13:09:48.811Z

Vulnerabilities in public API and auth-adjacent request handling, including trust of proxy-derived client identity and public-route abuse controls.

Vulnerabilities to fix:

1. [Medium] Clerk auth proxy trusts spoofable X-Forwarded-For
  The public authentication proxy lets internet clients lie about their source IP address. This weakens Clerk's built-in abuse protections, so an attacker can spread login, sign-up, or verification traffic across fake IPs and make brute-force or quota-abuse attacks much harder to stop.

`clerkProxyMiddleware()` reads the incoming `X-Forwarded-For` header directly from the request, takes the leftmost value, and forwards it to Clerk as the authoritative client IP:

- `artifacts/api-server/src/middlewares/clerkProxyMiddleware.ts:88-96`
- no trusted-proxy configuration exists in `artifacts/api-server/src/app.ts`

Because this deployment sits behind a reverse proxy, the application only receives proxy metadata through headers. However, the code does not use Express trusted-proxy handling and does not validate that the forwarded chain came from trusted infrastructure before consuming it. An attacker can send requests to `/api/__clerk/**` with a chosen header such as `X-Forwarded-For: 1.2.3.4`; if the platform appends its own proxy IP instead of replacing the header, the middleware still keeps the attacker's leftmost value and sends that to Clerk.

That makes the downstream Clerk Frontend API believe each request came from a different client. A practical exploitation path is to script repeated sign-in, sign-up, password-reset, or verification attempts while rotating fake `X-Forwarded-For` values. This bypasses the per-IP buckets Clerk uses for rate limiting and abuse detection, increasing the feasibility of credential stuffing and verification-channel abuse against the production auth surface. The route is unauthenticated and exposed on the public deployment, so no prior compromise is required.
  Files: artifacts/api-server/src/middlewares/clerkProxyMiddleware.ts, artifacts/api-server/src/app.ts

2. [Medium] Identify endpoint rate limit collapses all users into one proxy IP bucket
  A single attacker can temporarily block plant-identification lookups for everyone using the public app. The route tries to rate-limit by client IP, but in production it treats the deployment's reverse proxy as the client, so all users share the same quota bucket.

The unauthenticated `/api/identify` route uses `req.ip` as its rate-limit key and rejects requests after 40 hits per minute:

- `artifacts/api-server/src/routes/identify.ts:53-71`
- `artifacts/api-server/src/routes/identify.ts:105-110`

The Express app never enables `trust proxy` in `artifacts/api-server/src/app.ts`, so on a proxied autoscale deployment `req.ip` resolves to the reverse proxy's address rather than the real visitor IP. That means the in-memory `hits` map does not track individual callers; it tracks the shared ingress IP seen by the application.

An attacker can exploit this by sending more than 40 requests per minute to `/api/identify?name=...`. Once the shared proxy IP exceeds `RATE_MAX`, subsequent legitimate requests from other users are also keyed to that same bucket and receive `429 Too many requests`. Because the route is public and fans out to an external API, this can be sustained cheaply to keep the identification feature unavailable. The cache and timeout logic bound upstream cost, but they do not prevent this availability failure because the bug is in how the rate-limit identity is derived.
  Files: artifacts/api-server/src/routes/identify.ts, artifacts/api-server/src/app.ts

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #157 — Release Signing Integrity
- **Status:** MERGED
- **Created:** 2026-05-30T12:50:38.440Z
- **Updated:** 2026-05-30T13:28:27.589Z

Vulnerabilities in the iOS release pipeline that expose signing material and weaken the integrity of production mobile builds.

Vulnerabilities to fix:

1. [High] Workspace-stored iOS signing credentials allow unauthorized production build signing
  Anyone with access to the Replit workspace can take the app's real iPhone distribution certificate, its provisioning profile, and the password needed to unlock them, then use them to sign their own MossLight iOS builds as if they came from the legitimate publisher. This breaks release integrity because an unauthorized party can produce production-signed builds for the real app identifier.

The production EAS profile is explicitly configured to use local signing material (`artifacts/mobile/eas.json:28-37`), and the workflow invokes that profile for production iOS builds (`.replit:34-47`). The signing assets are present in an accessible workspace path: `artifacts/mobile/dist_cert.p12`, `artifacts/mobile/dist_profile.mobileprovision`, and `artifacts/mobile/credentials.json`. The password protecting the PKCS#12 bundle is recoverable in two places: `artifacts/mobile/credentials.json:2-7` and the hardcoded `P12_PASSWORD = "MossLightBuild2025"` in `scripts/setup-ios-credentials.mjs:15`. The same script intentionally copies freshly minted Apple signing artifacts into the workspace (`scripts/setup-ios-credentials.mjs:178-193`) and then skips regeneration whenever those files already exist (`scripts/setup-ios-credentials.mjs:28-35`), so the sensitive material persists instead of remaining ephemeral.

The exposed files are not dummy placeholders. The committed workspace currently contains a valid App Store distribution certificate and matching provisioning profile. Inspection of `artifacts/mobile/dist_cert.p12` with the published password shows a certificate for `iPhone Distribution: Michaela Walcott (QHS9VF3VX5)` issued by Apple, valid from 2026-05-30 to 2027-05-30. Inspection of `artifacts/mobile/dist_profile.mobileprovision` shows an App Store profile for `QHS9VF3VX5.com.quietgrovebotania.mosslightapp` with `aps-environment` set to `production`, team identifier `QHS9VF3VX5`, and the same 2027-05-30 expiry. In other words, a workspace reader can recover the exact private signing identity used for the production app.

Exploitation is straightforward: a collaborator, compromised agent session, or anyone else who can read workspace files can copy `dist_cert.p12`, `dist_profile.mobileprovision`, and the password from `credentials.json` or `setup-ios-credentials.mjs`, then use standard Apple tooling or EAS/local signing to build and sign arbitrary code for `com.quietgrovebotania.mosslightapp`. Even without separate App Store Connect submission credentials, this is still a real production-integrity issue because it enables unauthorized creation of publisher-signed artifacts, undermines trust in the release pipeline, and materially lowers the bar for any future compromise of TestFlight/App Store submission steps. The fix is to remove the signing bundle and provisioning profile from the workspace, rotate the exposed certificate/profile, and keep signing material only in dedicated secrets/CI storage rather than under `artifacts/mobile/`.
  Files: .replit, artifacts/mobile/eas.json, artifacts/mobile/credentials.json, artifacts/mobile/dist_cert.p12, artifacts/mobile/dist_profile.mobileprovision, scripts/setup-ios-credentials.mjs

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #158 — Shared-Device Sync and Account Transition Issues
- **Status:** MERGED
- **Created:** 2026-05-30T12:50:38.905Z
- **Updated:** 2026-05-30T13:16:04.016Z

Vulnerabilities in mobile account switching, snapshot restore, and profile-sync behavior that can expose one user’s data to another or contaminate cloud state across accounts.

Vulnerabilities to fix:

1. [Medium] Sign-in treats profile fetch failures as empty accounts and can overwrite another user's cloud data
  If the app cannot read the signed-in user's cloud profile during account switch, it behaves as if the account has no existing data. On a shared device, that can erase the new user's cloud backup or replace it with stale local data left behind by someone else.

In `artifacts/mobile/app/(auth)/sign-in.tsx`, both sign-in flows swallow `fetchProfile(...)` failures and then continue as though the account is empty. In the non-guest stale-device path, `incomingProfile` is left `null` when the fetch throws, and the code immediately falls into `clearLocalState()` (`sign-in.tsx:212-233`). In the guest-content path, `cloudData` is also left `null` on error, which makes `cloudHasRealContent` false and sends the user into the "Bring your garden?" flow intended only for genuinely empty accounts (`sign-in.tsx:256-420`). No distinction is made between a real `404` empty profile and transient network / 5xx failures.

That behavior becomes security-relevant because sync resumes afterward. `clearLocalState()` resets the local profile to empty defaults, and once `resumeSync()` is called the normal debounced background sync in `artifacts/mobile/context/GardenContext.tsx:776-800` can upload that empty snapshot to the newly signed-in account. In the guest path, choosing "Keep my plants" calls `forceSyncToCloud()` (`GardenContext.tsx:1327-1340`), which immediately uploads whatever local garden is on the device, even though the cloud lookup only failed and the account may already have unrelated data. A transient fetch failure therefore lets stale local state from a previous device user overwrite the next account, or lets an empty reset wipe that account's existing cloud snapshot.

A realistic exploitation scenario is: user A leaves local garden data on a shared device, user B signs into their own account while the `/profile` fetch fails temporarily, and the app either uploads A's local garden into B's account or clears B's local state and then syncs an empty snapshot over B's real cloud data once connectivity returns. Because the failure handling conflates "could not verify cloud state" with "cloud state is empty," the app loses the safety check that is supposed to prevent cross-user sync contamination.
  Files: artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/context/GardenContext.tsx

2. [High] Partial cloud restores leave prior user's garden visible to the next account
  When one person signs into the app on a device that still has another person's local garden data, the app can keep showing the earlier person's plants and stats after the new account is restored. This lets the next user view someone else's garden data and can also push that stale data into the wrong cloud account.

The non-guest account-switch path in `artifacts/mobile/app/(auth)/sign-in.tsx` activates the new Clerk session and, if the incoming account has any non-empty synced data, calls `restoreFromCloudSnapshot(...)` (`sign-in.tsx:206-239`). The restore helper in `artifacts/mobile/context/GardenContext.tsx:1485-1535` writes the incoming snapshot to AsyncStorage with `applySnapshot(snap)`, but it only updates many React state fields when the snapshot value is truthy. For example, `userName`, `userPlants`, `tier`, `totalWaterings`, `totalFertilizings`, `critterPrefs`, `points`, `friendPrefs`, `activeSkins`, and `purchasedPremiumSkins` are all conditionally assigned and are not reset to safe defaults when the incoming snapshot contains `null` for those keys.

That matters because synced snapshots can legitimately be partial from the app's point of view: a real account may contain only a name, only journal state, or other non-plant data. In that case `incomingHasData` is true, so the sign-in flow restores the cloud snapshot instead of clearing local state, but any missing fields stay populated from the previous device user's in-memory state. The next account can therefore see the previous user's plants, progression, and other retained state in the UI even though those keys were removed from AsyncStorage.

This is not just a display bug. Plant mutations such as `addPlant`, `removePlant`, `movePlant`, `waterPlant`, `fertilizePlant`, and `updateNotes` all call `persist(...)` (`GardenContext.tsx:968-1222`), which writes the currently displayed `userPlants` back to AsyncStorage. Once that happens, the normal background sync (`GardenContext.tsx:776-800`) or explicit sync paths can upload the prior user's stale garden into the newly signed-in user's cloud profile. A practical exploitation scenario is: user A leaves authenticated local state on a shared device, user B signs into an account that already has only partial cloud data (for example a stored name but no plants), and user B is then shown user A's plants and can unknowingly sync them into B's account.
  Files: artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/context/GardenContext.tsx

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #160 — Shared-Device Sync and Account Transition Issues
- **Status:** MERGED
- **Created:** 2026-05-30T12:57:05.395Z
- **Updated:** 2026-05-30T13:30:16.537Z

Vulnerabilities in mobile account switching, snapshot restore, and profile-sync behavior that can expose one user’s data to another or contaminate cloud state across accounts.

Vulnerabilities to fix:

1. [Medium] Sign-in treats profile fetch failures as empty accounts and can overwrite another user's cloud data
  If the app cannot read the signed-in user's cloud profile during account switch, it behaves as if the account has no existing data. On a shared device, that can erase the new user's cloud backup or replace it with stale local data left behind by someone else.

In `artifacts/mobile/app/(auth)/sign-in.tsx`, both sign-in flows swallow `fetchProfile(...)` failures and then continue as though the account is empty. In the non-guest stale-device path, `incomingProfile` is left `null` when the fetch throws, and the code immediately falls into `clearLocalState()` (`sign-in.tsx:212-233`). In the guest-content path, `cloudData` is also left `null` on error, which makes `cloudHasRealContent` false and sends the user into the "Bring your garden?" flow intended only for genuinely empty accounts (`sign-in.tsx:256-420`). No distinction is made between a real `404` empty profile and transient network / 5xx failures.

That behavior becomes security-relevant because sync resumes afterward. `clearLocalState()` resets the local profile to empty defaults, and once `resumeSync()` is called the normal debounced background sync in `artifacts/mobile/context/GardenContext.tsx:776-800` can upload that empty snapshot to the newly signed-in account. In the guest path, choosing "Keep my plants" calls `forceSyncToCloud()` (`GardenContext.tsx:1327-1340`), which immediately uploads whatever local garden is on the device, even though the cloud lookup only failed and the account may already have unrelated data. A transient fetch failure therefore lets stale local state from a previous device user overwrite the next account, or lets an empty reset wipe that account's existing cloud snapshot.

A realistic exploitation scenario is: user A leaves local garden data on a shared device, user B signs into their own account while the `/profile` fetch fails temporarily, and the app either uploads A's local garden into B's account or clears B's local state and then syncs an empty snapshot over B's real cloud data once connectivity returns. Because the failure handling conflates "could not verify cloud state" with "cloud state is empty," the app loses the safety check that is supposed to prevent cross-user sync contamination.
  Files: artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/context/GardenContext.tsx

2. [High] Partial cloud restores leave prior user's garden visible to the next account
  When one person signs into the app on a device that still has another person's local garden data, the app can keep showing the earlier person's plants and stats after the new account is restored. This lets the next user view someone else's garden data and can also push that stale data into the wrong cloud account.

The non-guest account-switch path in `artifacts/mobile/app/(auth)/sign-in.tsx` activates the new Clerk session and, if the incoming account has any non-empty synced data, calls `restoreFromCloudSnapshot(...)` (`sign-in.tsx:206-239`). The restore helper in `artifacts/mobile/context/GardenContext.tsx:1485-1535` writes the incoming snapshot to AsyncStorage with `applySnapshot(snap)`, but it only updates many React state fields when the snapshot value is truthy. For example, `userName`, `userPlants`, `tier`, `totalWaterings`, `totalFertilizings`, `critterPrefs`, `points`, `friendPrefs`, `activeSkins`, and `purchasedPremiumSkins` are all conditionally assigned and are not reset to safe defaults when the incoming snapshot contains `null` for those keys.

That matters because synced snapshots can legitimately be partial from the app's point of view: a real account may contain only a name, only journal state, or other non-plant data. In that case `incomingHasData` is true, so the sign-in flow restores the cloud snapshot instead of clearing local state, but any missing fields stay populated from the previous device user's in-memory state. The next account can therefore see the previous user's plants, progression, and other retained state in the UI even though those keys were removed from AsyncStorage.

This is not just a display bug. Plant mutations such as `addPlant`, `removePlant`, `movePlant`, `waterPlant`, `fertilizePlant`, and `updateNotes` all call `persist(...)` (`GardenContext.tsx:968-1222`), which writes the currently displayed `userPlants` back to AsyncStorage. Once that happens, the normal background sync (`GardenContext.tsx:776-800`) or explicit sync paths can upload the prior user's stale garden into the newly signed-in user's cloud profile. A practical exploitation scenario is: user A leaves authenticated local state on a shared device, user B signs into an account that already has only partial cloud data (for example a stored name but no plants), and user B is then shown user A's plants and can unknowingly sync them into B's account.
  Files: artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/context/GardenContext.tsx

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #161 — Security scan
- **Status:** MERGED
- **Created:** 2026-05-30T12:57:41.843Z
- **Updated:** 2026-05-30T16:55:52.285Z

Run an in-depth scan across the entire project

---

#### #162 — Fix shadow and pointerEvents deprecation warnings
- **Status:** MERGED
- **Created:** 2026-05-30T13:00:07.074Z
- **Updated:** 2026-05-30T13:37:20.872Z

# Fix React Native Deprecation Warnings

## What & Why
Two categories of deprecation warnings are showing up in dev logs. Cleaning them keeps the console clear so real issues are easy to spot and future React Native upgrades won't break these patterns.

## Done looks like
- No `shadow*` style prop warnings in the console (replaced with `boxShadow`)
- No `props.pointerEvents is deprecated` warnings (moved to `style.pointerEvents`)
- Visual appearance and behaviour are unchanged

## Out of scope
- `useNativeDriver` warnings — these are a web-preview-only limitation; animations work correctly on device and changing them would risk breaking native behaviour
- Clerk dev-key notice — expected in development, not a code issue
- Expo notifications web warning — platform limitation, not a code issue

## Steps
1. **Replace shadow style props with `boxShadow`** — In `PlantPreviewSheet.tsx`, `InfoTip.tsx`, `ErrorFallback.tsx`, `app/(tabs)/index.tsx`, and `app/(tabs)/discover.tsx`, convert each block of `shadowColor` / `shadowOffset` / `shadowOpacity` / `shadowRadius` into an equivalent `boxShadow` string value. Wrap in `Platform.select` if the shadow should only apply on iOS/Android (skip on web), or use a single `boxShadow` string that approximates the same look.

2. **Move `pointerEvents` from JSX prop to `style`** — In `app/(auth)/_layout.tsx`, `app/(tabs)/profile.tsx`, `app/(tabs)/_layout.tsx`, `components/PlantCard.tsx`, `components/CottageVisualizer.tsx`, `components/CottageCritters.tsx`, `components/TimelyMantisBadge.tsx`, `components/FriendIllustration.tsx`, `components/GreenhouseSceneSVG.tsx`, and `components/CottageBackground.tsx`, change every `pointerEvents="none"` / `pointerEvents="box-none"` JSX attribute into `style={{ pointerEvents: 'none' }}` (merged into the existing style prop where one is already present).

## Relevant files
- `artifacts/mobile/components/PlantPreviewSheet.tsx`
- `artifacts/mobile/components/InfoTip.tsx`
- `artifacts/mobile/components/ErrorFallback.tsx`
- `artifacts/mobile/app/(tabs)/index.tsx`
- `artifacts/mobile/app/(tabs)/discover.tsx`
- `artifacts/mobile/app/(auth)/_layout.tsx`
- `artifacts/mobile/app/(tabs)/profile.tsx`
- `artifacts/mobile/app/(tabs)/_layout.tsx`
- `artifacts/mobile/components/PlantCard.tsx`
- `artifacts/mobile/components/CottageVisualizer.tsx`
- `artifacts/mobile/components/CottageCritters.tsx`
- `artifacts/mobile/components/TimelyMantisBadge.tsx`
- `artifacts/mobile/components/FriendIllustration.tsx`
- `artifacts/mobile/components/GreenhouseSceneSVG.tsx`
- `artifacts/mobile/components/CottageBackground.tsx`

---

#### #164 — Shared-Device Sync and Account Transition Issues
- **Status:** MERGED
- **Created:** 2026-05-30T13:01:15.522Z
- **Updated:** 2026-05-30T16:33:17.331Z

Vulnerabilities in mobile account switching, snapshot restore, and profile-sync behavior that can expose one user’s data to another or contaminate cloud state across accounts.

Vulnerabilities to fix:

1. [Medium] Sign-in treats profile fetch failures as empty accounts and can overwrite another user's cloud data
  If the app cannot read the signed-in user's cloud profile during account switch, it behaves as if the account has no existing data. On a shared device, that can erase the new user's cloud backup or replace it with stale local data left behind by someone else.

In `artifacts/mobile/app/(auth)/sign-in.tsx`, both sign-in flows swallow `fetchProfile(...)` failures and then continue as though the account is empty. In the non-guest stale-device path, `incomingProfile` is left `null` when the fetch throws, and the code immediately falls into `clearLocalState()` (`sign-in.tsx:212-233`). In the guest-content path, `cloudData` is also left `null` on error, which makes `cloudHasRealContent` false and sends the user into the "Bring your garden?" flow intended only for genuinely empty accounts (`sign-in.tsx:256-420`). No distinction is made between a real `404` empty profile and transient network / 5xx failures.

That behavior becomes security-relevant because sync resumes afterward. `clearLocalState()` resets the local profile to empty defaults, and once `resumeSync()` is called the normal debounced background sync in `artifacts/mobile/context/GardenContext.tsx:776-800` can upload that empty snapshot to the newly signed-in account. In the guest path, choosing "Keep my plants" calls `forceSyncToCloud()` (`GardenContext.tsx:1327-1340`), which immediately uploads whatever local garden is on the device, even though the cloud lookup only failed and the account may already have unrelated data. A transient fetch failure therefore lets stale local state from a previous device user overwrite the next account, or lets an empty reset wipe that account's existing cloud snapshot.

A realistic exploitation scenario is: user A leaves local garden data on a shared device, user B signs into their own account while the `/profile` fetch fails temporarily, and the app either uploads A's local garden into B's account or clears B's local state and then syncs an empty snapshot over B's real cloud data once connectivity returns. Because the failure handling conflates "could not verify cloud state" with "cloud state is empty," the app loses the safety check that is supposed to prevent cross-user sync contamination.
  Files: artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/context/GardenContext.tsx

2. [High] Partial cloud restores leave prior user's garden visible to the next account
  When one person signs into the app on a device that still has another person's local garden data, the app can keep showing the earlier person's plants and stats after the new account is restored. This lets the next user view someone else's garden data and can also push that stale data into the wrong cloud account.

The non-guest account-switch path in `artifacts/mobile/app/(auth)/sign-in.tsx` activates the new Clerk session and, if the incoming account has any non-empty synced data, calls `restoreFromCloudSnapshot(...)` (`sign-in.tsx:206-239`). The restore helper in `artifacts/mobile/context/GardenContext.tsx:1485-1535` writes the incoming snapshot to AsyncStorage with `applySnapshot(snap)`, but it only updates many React state fields when the snapshot value is truthy. For example, `userName`, `userPlants`, `tier`, `totalWaterings`, `totalFertilizings`, `critterPrefs`, `points`, `friendPrefs`, `activeSkins`, and `purchasedPremiumSkins` are all conditionally assigned and are not reset to safe defaults when the incoming snapshot contains `null` for those keys.

That matters because synced snapshots can legitimately be partial from the app's point of view: a real account may contain only a name, only journal state, or other non-plant data. In that case `incomingHasData` is true, so the sign-in flow restores the cloud snapshot instead of clearing local state, but any missing fields stay populated from the previous device user's in-memory state. The next account can therefore see the previous user's plants, progression, and other retained state in the UI even though those keys were removed from AsyncStorage.

This is not just a display bug. Plant mutations such as `addPlant`, `removePlant`, `movePlant`, `waterPlant`, `fertilizePlant`, and `updateNotes` all call `persist(...)` (`GardenContext.tsx:968-1222`), which writes the currently displayed `userPlants` back to AsyncStorage. Once that happens, the normal background sync (`GardenContext.tsx:776-800`) or explicit sync paths can upload the prior user's stale garden into the newly signed-in user's cloud profile. A practical exploitation scenario is: user A leaves authenticated local state on a shared device, user B signs into an account that already has only partial cloud data (for example a stored name but no plants), and user B is then shown user A's plants and can unknowingly sync them into B's account.
  Files: artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/context/GardenContext.tsx

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #165 — Add accessibility support (VoiceOver, Larger Text, Dark Interface, Reduced Motion, Differentiate Without Color)
- **Status:** MERGED
- **Created:** 2026-05-30T13:15:37.545Z
- **Updated:** 2026-05-30T13:38:32.851Z

# Accessibility Support (App Store)

## What & Why
Add the six accessibility features required to claim App Store accessibility support: VoiceOver, Voice Control, Larger Text, Dark Interface, Differentiate Without Color Alone, and Reduced Motion. Captions and Audio Descriptions are out of scope (no video/audio content).

## Done looks like
- Every interactive element across all tab screens and key modals has an `accessibilityLabel`, `accessibilityRole`, and where needed an `accessibilityHint` — navigable end-to-end with VoiceOver/Voice Control
- All `Text` components allow font scaling up to at least 200% without layout breakage
- A system dark mode preference automatically selects a dark theme (e.g. Conservatory) on first launch if the device is in dark mode, without overriding a user's manual theme choice
- Status and care indicators (watering due, overdue, healthy) use icons or text labels in addition to color — not color alone
- All animations (tab entrance, celebration burst, screen flash, undo banner, welcome slides) check `AccessibilityInfo.isReduceMotionEnabled()` and are skipped or replaced with an instant fade when enabled
- The app can be submitted to App Store Connect with VoiceOver, Voice Control, Larger Text, Dark Interface, Differentiate Without Color Alone, and Reduced Motion all checked

## Out of scope
- Captions
- Audio Descriptions
- WCAG AAA contrast (AA sufficient)
- New themes or visual redesign

## Steps
1. **VoiceOver & Voice Control labels** — Audit all tab screens (Greenhouse, Conservatory, Journal, Herbarium, Profile), key modals (Add Plant, Plant Detail, Species Detail), and shared components (PlantCard, PlantPreviewSheet, InfoTip, CelebrationBurst). Add `accessibilityLabel`, `accessibilityRole`, and `accessibilityHint` to every interactive element. Group decorative elements with `accessible={false}` to reduce noise.

2. **Larger Text (Dynamic Type)** — Remove any `allowFontScaling={false}` instances. Set a sensible `maxFontSizeMultiplier` (e.g. 1.5–2.0) on Text components inside constrained layouts (cards, tab bar labels) so they scale gracefully without breaking the UI.

3. **Dark Interface** — On first launch (or when no explicit theme has been chosen), read the system `colorScheme` via `useColorScheme()` and default to a dark theme (Conservatory or Midnight Ink) if the device is in dark mode. Respect any subsequent manual theme selection the user makes in Profile.

4. **Differentiate Without Color Alone** — Audit care-status indicators (watering overdue, due today, healthy) and any badges or progress indicators that currently rely on color alone. Add an icon or short text label alongside each so the state is legible without color perception.

5. **Reduced Motion** — Create a `useReducedMotion()` hook that wraps `AccessibilityInfo.isReduceMotionEnabled()` and subscribes to changes. Apply it to: tab entrance animation, CelebrationBurst particle system, ScreenFlash, undo banner, and WelcomeScreen slide transitions — skipping or replacing each with a simple opacity fade when enabled.

## Relevant files
- `artifacts/mobile/constants/themes.ts`
- `artifacts/mobile/contexts/ThemeContext.tsx`
- `artifacts/mobile/app/(tabs)/index.tsx`
- `artifacts/mobile/app/(tabs)/garden.tsx`
- `artifacts/mobile/app/(tabs)/journal.tsx`
- `artifacts/mobile/app/(tabs)/discover.tsx`
- `artifacts/mobile/app/(tabs)/profile.tsx`
- `artifacts/mobile/app/(tabs)/_layout.tsx`
- `artifacts/mobile/app/add-plant.tsx`
- `artifacts/mobile/app/plant/[id].tsx`
- `artifacts/mobile/app/encyclopedia/[speciesId].tsx`
- `artifacts/mobile/components/ErrorFallback.tsx`
- `artifacts/mobile/components/InfoTip.tsx`

---

#### #167 — Release Signing Integrity
- **Status:** MERGED
- **Created:** 2026-05-31T04:40:12.458Z
- **Updated:** 2026-05-31T05:04:55.989Z

Vulnerabilities in the iOS release pipeline that expose reusable signing capability outside dedicated secrets or CI controls.

Vulnerabilities to fix:

1. [High] Live workspace build steps expose reusable iOS signing material
  Anyone who can read files from the live Replit workspace during or after a production iOS build can recover the certificate, provisioning profile, and password needed to sign their own MossLight app builds. That lets an insider or compromised workspace session create publisher-signed iOS binaries for the real production bundle identifier.

The previous repository exposure has been reduced, but the current design still materializes the signing set into predictable local paths during every production build. The production workflow in `.replit:34-47` always runs `node scripts/write-p8.mjs && node scripts/setup-ios-credentials.mjs` before `eas build`, and `artifacts/mobile/eas.json:28-37` forces the production profile to use `credentialsSource: "local"`. `scripts/setup-ios-credentials.mjs` then writes the generated signing assets to fixed locations in the same build container: `/tmp/dist_cert.p12`, `/tmp/dist_profile.mobileprovision`, and `/tmp/ios_p12_password.txt` (`scripts/setup-ios-credentials.mjs:30-55`, `145-151`, `198-206`). It also writes `artifacts/mobile/credentials.json` in the workspace with the PKCS#12 password and the absolute paths to those `/tmp` files (`scripts/setup-ios-credentials.mjs:27-28`, `90-100`).

This is enough for a live workspace reader to recover production signing capability. The script intentionally reuses any existing `/tmp` signing files in later runs (`scripts/setup-ios-credentials.mjs:40-49`), so the material can persist across builds in the same container instead of existing only for a single command invocation. A collaborator, compromised agent session, or other actor with read access to the active Replit filesystem can copy `artifacts/mobile/credentials.json`, read the referenced `/tmp` files, and then use standard Apple tooling or local EAS signing to produce arbitrary binaries for `com.quietgrovebotania.mosslightapp`. Even without separate App Store Connect submission credentials, that still breaks release integrity because it enables unauthorized creation of valid publisher-signed app artifacts.
  Files: .replit, artifacts/mobile/eas.json, scripts/setup-ios-credentials.mjs

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #168 — Shared-Device Sync and Account Transition Issues
- **Status:** MERGED
- **Created:** 2026-05-31T04:40:12.510Z
- **Updated:** 2026-05-31T05:11:31.741Z

Vulnerabilities where retained local profile state can cross from one person or session into another account on the same device.

Vulnerabilities to fix:

1. [High] Guest journal and check-in data can sync into the next signed-in account
  A shared device user can create guest-only journal or check-in data without adding any plants, then have that data silently attached to the next person who signs in on the same device. This exposes one person's private notes and activity to another account and can also overwrite that account's cloud backup with stale guest data.

The guest sign-in flow in `artifacts/mobile/app/(auth)/sign-in.tsx` decides whether guest data needs conflict handling by checking only `userPlants.some(isRealPlant)` (`sign-in.tsx:257-263`). If there are no real plants, it immediately activates the new session and returns without clearing local state and without fetching the incoming account's cloud profile. That is inconsistent with the file's own `snapshotHasAnyData(...)` helper (`sign-in.tsx:43-60`), which correctly recognizes that synced profile state includes non-plant data such as journal entries, read state, settings, and other keys.

This matters because guests can create synced data without owning any plants. The journal screen lets an empty garden write a first entry directly from the empty state (`artifacts/mobile/app/(tabs)/journal.tsx:233-255`) and saves it to `@quietgrove:journalEntries` (`artifacts/mobile/hooks/useJournal.ts:12-29,31-45`). The same screen also exposes a daily check-in action even with zero plants (`journal.tsx:207-255`), and `useDailyPrompt` stores that to `@quietgrove:lastCheckIn` (`artifacts/mobile/hooks/useDailyPrompt.ts:5,27-32`). Those keys are part of `ALL_PROFILE_KEYS` (`artifacts/mobile/utils/api.ts:7-37`), so once sign-in flips the session out of guest mode, the normal background sync in `artifacts/mobile/context/GardenContext.tsx:776-804` will upload that stale guest snapshot to the newly signed-in account.

An exploit path is straightforward on a family tablet or other reused device: user A opens the app as a guest, writes personal journal notes or checks in for the day without adding any plants, then leaves. User B later signs into their own Clerk account on the same device. Because `hasGuestContent` only looks at plants, the app skips cleanup and cloud conflict detection, keeps A's local guest snapshot in place, and shortly afterward syncs it into B's account. If B already had cloud data, the first sync can overwrite the server copy with A's guest-origin journal or other non-plant state. This is a production-reachable cross-account data exposure and tampering issue on shared devices.
  Files: artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/app/(tabs)/journal.tsx, artifacts/mobile/context/GardenContext.tsx, artifacts/mobile/hooks/useDailyPrompt.ts, artifacts/mobile/hooks/useJournal.ts, artifacts/mobile/utils/api.ts

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #169 — Security scan
- **Status:** MERGED
- **Created:** 2026-06-02T06:44:33.699Z
- **Updated:** 2026-06-02T06:57:14.627Z

Run an in-depth scan across the entire project

---

#### #170 — Release Pipeline Credentials
- **Status:** MERGED
- **Created:** 2026-06-02T06:52:37.846Z
- **Updated:** 2026-06-02T17:25:49.338Z

Production Apple private keys and related release automation exposures that can let a repository reader interfere with iOS release integrity or monetization integrations.

Vulnerabilities to fix:

1. [High] Committed App Store Connect build key enables release-signing abuse
  A private Apple signing key used by the iOS release workflow is stored directly in the repository. Anyone who can read the repo can extract that key and use it to interfere with production iOS signing assets or drive unauthorized release-related actions against the Apple account.

The production build workflow in `.replit` invokes `node scripts/write-p8.mjs && node scripts/setup-ios-credentials.mjs` before running `eas build` (`.replit:31-33`). The same file also hard-codes the private key material in `ASC_KEY_P8_OVERRIDE` and its key identifier in `ASC_KEY_ID_OVERRIDE` (`.replit:117-118`). `scripts/write-p8.mjs` explicitly prefers `ASC_KEY_P8_OVERRIDE` over any secret-managed value and writes it to `/tmp/asc_key.p8` (`scripts/write-p8.mjs:4-29`). `scripts/setup-ios-credentials.mjs` then uses that key, along with hard-coded issuer/key defaults, to mint App Store Connect JWTs and call privileged Apple endpoints including `/v1/certificates`, `/v1/profiles`, `/v1/bundleIds`, and `/v1/bundleIdCapabilities` (`scripts/setup-ios-credentials.mjs:24-29`, `67-99`, `144-177`, `203-234`, `261-317`).

This means the repository itself contains a live App Store Connect API private key that is sufficient for the release pipeline's certificate/profile management. A repository reader does not need any separate secret store access to reproduce those requests: they can extract the PEM from `.replit`, pair it with the committed key ID/issuer values, mint their own ES256 JWT, and invoke the same Apple APIs. Practical abuse includes revoking or churning iOS distribution certificates to break future releases, generating attacker-controlled signing material for the MossLight bundle IDs, and otherwise tampering with the integrity and availability of the iOS release process. Because this credential is actively wired into the production build workflow, the exposure is not theoretical.
  Files: .replit, scripts/write-p8.mjs, scripts/setup-ios-credentials.mjs

2. [High] Committed Apple subscription key exposes App Store monetization integration
  A second private Apple API key used for the app's subscription and receipt-verification integration is also committed in the repository. A repo reader can take that key and act against the App Store Connect side of MossLight's monetization setup without needing access to the normal secret store.

`.replit` stores `RC_APPLE_ISSUER_ID`, `RC_APPLE_SUBSCRIPTION_KEY_ID`, and the full `RC_APPLE_SUBSCRIPTION_KEY_P8` private key in plaintext (`.replit:107-115`). `scripts/src/configureAppleKey.ts` reads those values and sends them to RevenueCat as the App Store Connect API key for the production App Store app (`scripts/src/configureAppleKey.ts:8-18`, `35-45`). The script comment identifies this as the key used for receipt verification and product synchronization for the App Store integration (`scripts/src/configureAppleKey.ts:1-3`).

Because the private key itself is committed, anyone with repository read access can mint valid App Store Connect JWTs for that Apple API key. Even if this key is narrower than the build-signing key, it still protects a production billing integration: an attacker can at minimum target the App Store Connect side of subscription/product synchronization and associated monetization workflows, and can disrupt or manipulate the configuration that RevenueCat relies on for receipt handling. This is a true secret exposure, unlike the `EXPO_PUBLIC_REVENUECAT_*` values in `artifacts/mobile/eas.json`, which are publishable SDK keys intentionally shipped to clients.
  Files: .replit, scripts/src/configureAppleKey.ts

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #171 — Shared-Device Web Storage
- **Status:** MERGED
- **Created:** 2026-06-02T06:52:37.954Z
- **Updated:** 2026-06-02T17:58:06.057Z

Vulnerabilities where retained browser or device-local state exposes one user's data to the next human using the same client environment.

Vulnerabilities to fix:

1. [Medium] Signed-out web sessions retain prior user's full garden profile in browser storage
  On the web app, signing out is not the only way a session ends: closing the tab, letting the Clerk session disappear, or deleting the account can leave the previous person's garden data behind in browser storage. The next person using the same browser can recover the previous user's plants, notes, journal state, and related profile data from local storage even though the app shows a signed-out screen.

The root cause is that authenticated local profile state is always rehydrated from AsyncStorage before any auth check, but only stale guest sessions are auto-wiped. `GardenContext` eagerly loads all persisted profile keys, including `@quietgrove:userPlants`, `@quietgrove:journalEntries`, `@quietgrove:learnRead`, `@quietgrove:speciesRead`, `@quietgrove:greenhouse_code`, and other synced fields (`artifacts/mobile/context/GardenContext.tsx:453-572`, `artifacts/mobile/utils/api.ts:7-37`). It explicitly wipes abandoned guest sessions (`artifacts/mobile/context/GardenContext.tsx:517-551`), but there is no equivalent cleanup for abandoned authenticated sessions. The UI auth gate in `artifacts/mobile/app/(tabs)/_layout.tsx:541-572` only redirects to `/sign-in`; it does not clear the already-hydrated state.

This becomes exploitable on web because Clerk tokens are intentionally stored in `sessionStorage` and disappear when the tab closes (`artifacts/mobile/app/_layout.tsx:59-88`), while the app's profile snapshot remains in persistent AsyncStorage-backed browser storage. After a tab close or session expiry, the next person opening the same browser is unauthenticated but the previous user's garden snapshot is still present under predictable `@quietgrove:*` keys. A local user can read that data directly from browser storage or devtools without knowing the previous account credentials. The sign-in screen itself confirms stale data is present by rendering a "plants ready to back up" banner from `userPlants` even when the viewer is not the prior account owner (`artifacts/mobile/app/(auth)/sign-in.tsx:595-618`).

The account-deletion flow makes this worse. `handleDeleteAccount` calls `await user.delete(); router.replace("/(auth)/sign-in")` without invoking `clearLocalState()` first (`artifacts/mobile/app/(tabs)/profile.tsx:497-520`), so a user who deletes their account on a shared browser still leaves their local profile snapshot behind for the next browser user to inspect.

A practical attack is straightforward: User A signs in to the public web deployment from a shared browser, uses the app, then closes the tab or deletes the account. User B later opens the same browser to the MossLight origin. Even though the app is signed out, User B can inspect the origin's stored `@quietgrove:*` values and recover User A's plant list, notes embedded in `userPlants`, journal entries, reminder preferences, and other synced profile state. This is a cross-user disclosure within the shared-device threat model the app explicitly calls out.
  Files: artifacts/mobile/app/_layout.tsx, artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/app/(tabs)/_layout.tsx, artifacts/mobile/app/(tabs)/profile.tsx, artifacts/mobile/context/GardenContext.tsx, artifacts/mobile/utils/api.ts

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #172 — Security scan
- **Status:** MERGED
- **Created:** 2026-06-04T12:58:41.435Z
- **Updated:** 2026-06-04T13:22:22.153Z

Run an in-depth scan across the entire project

---

#### #173 — Public Runtime Availability
- **Status:** MERGED
- **Created:** 2026-06-04T13:21:35.558Z
- **Updated:** 2026-06-04T13:37:59.258Z

Vulnerabilities in internet-facing production entry points that allow unauthenticated disruption of the deployed application.

Vulnerabilities to fix:

1. [High] Malformed Host header crashes the public root server
  A malformed `Host` header can make MossLight's public root server crash before it serves any content. This means an unauthenticated attacker on the internet can repeatedly knock the `/` service offline and disrupt access to the landing page, Expo manifest, and static build assets.

The production mobile artifact is served by `node artifacts/mobile/server/serve.js` according to `artifacts/mobile/.replit-artifact/artifact.toml`. In that server, every request is parsed with `new URL(req.url || "/", \`http://${req.headers.host}\`)` at `artifacts/mobile/server/serve.js:112` before any validation or error handling. Node accepts requests whose `Host` header contains values such as `%zz`, but `new URL()` throws `TypeError: Invalid URL` for that base. Because the exception is unhandled inside the request callback, the Node process exits.

This is reachable from the public internet with a single raw HTTP request; no authentication or browser interaction is required. During verification, sending `GET / HTTP/1.1` with `Host: %zz` caused the same code path to terminate the process with `ERR_INVALID_URL`. An attacker can automate these requests to keep the root service in a crash-loop, degrading or denying access to `/`, `/manifest`, and all static files served by this process. The API service is separate, but the public runtime described in the session plan still loses availability.

Root cause: the server treats `req.headers.host` as trusted input for URL construction even though request headers are attacker-controlled at this trust boundary, and it does so without sanitization or a defensive `try/catch`. The same area also trusts `x-forwarded-host` and `x-forwarded-proto` for HTML generation, which reinforces that header handling in this entry point is unsafe.
  Files: artifacts/mobile/server/serve.js, artifacts/mobile/.replit-artifact/artifact.toml

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #174 — Release Pipeline Credentials
- **Status:** MERGED
- **Created:** 2026-06-04T13:21:35.823Z
- **Updated:** 2026-06-04T13:35:43.976Z

Vulnerabilities that expose production mobile signing or submission material and can undermine release integrity.

Vulnerabilities to fix:

1. [High] Production iOS signing key and provisioning profile left in workspace
  Production iOS release credentials were materialized into the project workspace and left there after use. Anyone who can read the workspace can recover the MossLight App Store signing private key and provisioning profile, which undermines the integrity of future iOS releases.

The release pipeline intentionally writes live signing material into reusable project paths under `artifacts/mobile`: `scripts/setup-ios-credentials.mjs` creates `artifacts/mobile/.eas-creds/dist_cert.p12`, `artifacts/mobile/.eas-creds/dist_profile.mobileprovision`, and `artifacts/mobile/credentials.json` containing the p12 password (`scripts/setup-ios-credentials.mjs:43-46`, `295-316`). Although `.replit` attempts to delete these files after `eas build` finishes (`.replit:31-33`), the credentials are currently still present in the workspace, which proves the cleanup is best-effort rather than guaranteed.

This is a concrete exposure, not a hypothetical one. `artifacts/mobile/credentials.json` contains the decryption password for `.eas-creds/dist_cert.p12`, and that bundle decrypts to a private key and certificate for `iPhone Distribution: Michaela Walcott (QHS9VF3VX5)` valid until `2027-06-04`. The accompanying provisioning profile is an App Store profile for `QHS9VF3VX5.com.quietgrovebotania.mosslightapp`, also valid until `2027-06-04`, with team `QHS9VF3VX5` and app name `MossLight App`. In other words, a workspace reader can extract the real production signing identity for the shipped iOS app.

An attacker with repository/workspace access could reuse these artifacts to sign a malicious MossLight iOS build for the production bundle identifier, replace expected build outputs, or prepare a trojanized release for submission once they obtain any App Store Connect submission path. Even without the App Store Connect API private key, exposure of the signing private key and matching provisioning profile is a high-impact release-integrity failure because it gives attackers the same cryptographic identity used by the legitimate production iOS release process. The root cause is storing live signing material in reusable workspace paths instead of keeping it exclusively in ephemeral secrets-backed storage with guaranteed cleanup.
  Files: .replit, scripts/setup-ios-credentials.mjs, artifacts/mobile/credentials.json, artifacts/mobile/.eas-creds/dist_cert.p12, artifacts/mobile/.eas-creds/dist_profile.mobileprovision

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #175 — Fix 5 dependency vulnerabilities
- **Status:** MERGED
- **Created:** 2026-06-04T13:21:38.278Z
- **Updated:** 2026-06-04T13:41:26.885Z

Fix the following dependency vulnerabilities:

- [High] @xmldom/xmldom@0.7.13 (GHSA-2v35-w6hq-6mfw@@xmldom/xmldom-0.7.13)
- [High] @xmldom/xmldom@0.7.13 (GHSA-f6ww-3ggp-fr8h@@xmldom/xmldom-0.7.13)
- [High] @xmldom/xmldom@0.7.13 (GHSA-j759-j44w-7fr8@@xmldom/xmldom-0.7.13)
- [High] @xmldom/xmldom@0.7.13 (GHSA-wh4c-j3r5-mjhp@@xmldom/xmldom-0.7.13)
- [High] @xmldom/xmldom@0.7.13 (GHSA-x6wf-f3px-wcqx@@xmldom/xmldom-0.7.13)

---

#### #178 — Public Runtime Availability
- **Status:** MERGED
- **Created:** 2026-06-04T13:37:48.625Z
- **Updated:** 2026-06-05T13:27:35.072Z

Vulnerabilities in internet-facing production entry points that allow unauthenticated disruption of the deployed application.

Vulnerabilities to fix:

1. [High] Malformed Host header crashes the public root server
  A malformed `Host` header can make MossLight's public root server crash before it serves any content. This means an unauthenticated attacker on the internet can repeatedly knock the `/` service offline and disrupt access to the landing page, Expo manifest, and static build assets.

The production mobile artifact is served by `node artifacts/mobile/server/serve.js` according to `artifacts/mobile/.replit-artifact/artifact.toml`. In that server, every request is parsed with `new URL(req.url || "/", \`http://${req.headers.host}\`)` at `artifacts/mobile/server/serve.js:112` before any validation or error handling. Node accepts requests whose `Host` header contains values such as `%zz`, but `new URL()` throws `TypeError: Invalid URL` for that base. Because the exception is unhandled inside the request callback, the Node process exits.

This is reachable from the public internet with a single raw HTTP request; no authentication or browser interaction is required. During verification, sending `GET / HTTP/1.1` with `Host: %zz` caused the same code path to terminate the process with `ERR_INVALID_URL`. An attacker can automate these requests to keep the root service in a crash-loop, degrading or denying access to `/`, `/manifest`, and all static files served by this process. The API service is separate, but the public runtime described in the session plan still loses availability.

Root cause: the server treats `req.headers.host` as trusted input for URL construction even though request headers are attacker-controlled at this trust boundary, and it does so without sanitization or a defensive `try/catch`. The same area also trusts `x-forwarded-host` and `x-forwarded-proto` for HTML generation, which reinforces that header handling in this entry point is unsafe.
  Files: artifacts/mobile/server/serve.js, artifacts/mobile/.replit-artifact/artifact.toml

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #179 — Security scan
- **Status:** MERGED
- **Created:** 2026-06-06T04:41:36.212Z
- **Updated:** 2026-06-06T10:33:27.748Z

Run an in-depth scan across the entire project

---

#### #180 — Proxy and Rate Limiting
- **Status:** MERGED
- **Created:** 2026-06-06T10:32:36.949Z
- **Updated:** 2026-06-06T10:39:17.946Z

Vulnerabilities caused by incorrect client identity derivation through the deployed proxy chain.

Vulnerabilities to fix:

1. [Medium] Incorrect proxy trust breaks IP-based abuse controls on public identify and Clerk proxy traffic
  The production server is deriving caller IP addresses from the wrong proxy hop, so its public abuse controls do not actually recognize repeated requests from the same attacker. In practice, the unauthenticated plant-identification endpoint accepted more than its documented limit of 40 requests per minute from a single client, which means attackers can drive far more traffic and upstream work than intended.

`artifacts/api-server/src/app.ts` sets `app.set("trust proxy", 1)` based on the assumption that production has exactly one trusted reverse-proxy hop. That IP is then used in two security-sensitive places: `artifacts/api-server/src/routes/identify.ts` applies its only rate limit with `const ip = req.ip ?? req.socket.remoteAddress ?? "unknown"`, and `artifacts/api-server/src/middlewares/clerkProxyMiddleware.ts` forwards the same derived value to Clerk as `X-Forwarded-For` for abuse detection. The deployed service is visibly behind Google Frontend (`server: Google Frontend`, `via: 1.1 google` in responses), so the single-hop trust assumption is not reliable for production traffic.

When Express trusts fewer proxy hops than actually exist, `req.ip` resolves to the nearest untrusted proxy rather than the real client. That makes IP-based controls ineffective: requests from one attacker can be attributed to varying ingress addresses instead of the attacker’s stable source. I confirmed this against the public deployment at `https://MossLight.replit.app` by sending 55 sequential requests within one minute to `/api/identify?name=Monstera%20deliciosa`; every request returned HTTP 200, even though the route’s code is supposed to return HTTP 429 after 40 requests per minute. This is concrete evidence that the production rate limit can be bypassed.

The impact is twofold. First, `/api/identify` is an unauthenticated endpoint that can trigger external iNaturalist lookups on cache misses, so attackers can generate substantially more public work than intended and increase the risk of upstream quota exhaustion or service degradation. Second, the Clerk proxy forwards the same mis-derived IP to Clerk, which weakens Clerk’s per-client abuse bucketing and lockout logic by feeding it proxy addresses instead of the real caller. An attacker does not need credentials to exploit the public identify endpoint; they only need to send repeated requests to the production URL.
  Files: artifacts/api-server/src/app.ts, artifacts/api-server/src/routes/identify.ts, artifacts/api-server/src/middlewares/clerkProxyMiddleware.ts

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #181 — Release Signing Credentials
- **Status:** MERGED
- **Created:** 2026-06-06T10:32:37.064Z
- **Updated:** 2026-06-06T10:51:30.716Z

Vulnerabilities that expose production mobile signing material or weaken release integrity.

Vulnerabilities to fix:

1. [High] Production iOS signing credentials persist in predictable workspace files
  Production iOS release credentials are being left behind in ordinary workspace files instead of staying only in secret storage. Anyone with workspace/container read access can recover the certificate, provisioning profile, and password needed to sign a MossLight iOS release, which undermines the integrity of future production builds.

`scripts/setup-ios-credentials.mjs` intentionally writes the Apple distribution certificate bundle to `artifacts/mobile/.eas-creds/dist_cert.p12`, the App Store provisioning profile to `artifacts/mobile/.eas-creds/dist_profile.mobileprovision`, and the matching decryption password into `artifacts/mobile/credentials.json` (`scripts/setup-ios-credentials.mjs:30-45`, `66`, `299-320`). The script comments acknowledge these files are placed in the project tree so EAS will archive them for remote builds (`scripts/setup-ios-credentials.mjs:6-11`, `322-328`). `artifacts/mobile/.easignore` does not exclude `.eas-creds/` or `credentials.json`, so those secrets remain part of the build context sent from the workspace.

This is not just theoretical. The current workspace already contains all three pieces of live signing material: `artifacts/mobile/credentials.json` includes the plaintext password `d2cf0b649a72b10f5865141730a16141`, `artifacts/mobile/.eas-creds/dist_cert.p12` is present, and `artifacts/mobile/.eas-creds/dist_profile.mobileprovision` is present. Using that password to inspect the p12 shows an active Apple Distribution certificate for team `QHS9VF3VX5` with subject `iPhone Distribution: Michaela Walcott (QHS9VF3VX5)`, valid until `2027-06-06`. Parsing the provisioning profile shows `application-identifier: QHS9VF3VX5.com.quietgrovebotania.mosslightapp`, confirming the bundle is for the production MossLight iOS app.

Although `artifacts/mobile/.gitignore` says these files are sensitive and should never be committed, that only protects the Git history. It does not protect the actual Replit workspace/container, where the files currently exist and are readable by anyone with workspace access. The cleanup logic is also not reliable as an isolation boundary: `.replit` only wraps one build workflow with best-effort deletion, while other credential-writing paths such as `scripts/eas-build.mjs` and `scripts/write-p8.mjs` also materialize Apple credentials locally. The presence of the signing bundle in the workspace demonstrates that production signing material can persist after release operations.

An attacker or overly-privileged workspace reader can copy `artifacts/mobile/credentials.json`, `artifacts/mobile/.eas-creds/dist_cert.p12`, and the provisioning profile, then use them to sign a malicious IPA for the MossLight bundle identifier or replace a legitimate build artifact before upload/submission. Even if separate App Store upload credentials are stored elsewhere, exposure of the production signing identity is enough to compromise release integrity and to prepare tampered artifacts that appear to come from the legitimate developer team.
  Files: .replit, scripts/setup-ios-credentials.mjs, scripts/write-p8.mjs, scripts/eas-build.mjs, artifacts/mobile/credentials.json, artifacts/mobile/.eas-creds/dist_cert.p12, artifacts/mobile/.eas-creds/dist_profile.mobileprovision, artifacts/mobile/.easignore

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #182 — Shared Device Isolation
- **Status:** MERGED
- **Created:** 2026-06-06T10:32:37.176Z
- **Updated:** 2026-06-06T10:47:10.084Z

Vulnerabilities where retained local app state can leak between different humans using the same device.

Vulnerabilities to fix:

1. [Medium] Abandoned guest sessions stay live and can be synced into another user's account
  If one person uses MossLight in guest mode and leaves the app running on a shared device, the next person can immediately see that guest's saved garden data and can upload it into their own cloud account. This breaks the app's shared-device isolation promise and can expose or mix private plant notes, journal data, and activity history between different people.

The guest-session cleanup only happens during initial hydration after a cold start, when `GardenContext` sees a persisted `GUEST_KEY` and wipes storage (`artifacts/mobile/context/GardenContext.tsx:575-609`). There is no equivalent cleanup when a guest session is merely backgrounded and later resumed. The only `AppState` handler in the provider is for pending cloud sync on non-guests (`artifacts/mobile/context/GardenContext.tsx:844-856`), and tab access remains allowed whenever `isGuest` is still true (`artifacts/mobile/app/(tabs)/_layout.tsx:541-572`). As a result, a guest user's in-memory state survives normal app switching / device handoff and stays fully accessible to the next human who opens the app again.

That retained state is not limited to visible plants. The synced profile snapshot includes journal entries, read-state, custom locations, reminder settings, greenhouse code cache, and other user-scoped keys via `ALL_PROFILE_KEYS` and `buildSnapshot()` (`artifacts/mobile/utils/api.ts:7-50`). Once the second user reaches the sign-in screen from the still-live guest session, the `isGuest === true` path in `sign-in.tsx` treats the retained local state as intentional guest content. If the new account has no cloud profile, the app offers “Bring your garden?” and `Keep my plants` calls `forceSyncToCloud()` (`artifacts/mobile/app/(auth)/sign-in.tsx:154-169,467-468`). If the new account already has cloud data, the same guest snapshot can be pushed through the conflict-resolution paths (`keep_local` / `merge`) (`artifacts/mobile/app/(auth)/sign-in.tsx:305-358,367-452,664-681`).

A practical exploit is straightforward on a family tablet or other shared device: User A taps "Sign up later," adds plants and notes, then leaves the app open or backgrounded. User B later opens the still-running app, sees A's garden immediately, and can sign in with their own account and choose to keep or merge the local garden. MossLight then uploads User A's retained guest snapshot into User B's account. The existing cold-start wipe reduces exposure after a full restart, but it does not protect the common production flow where the app remains alive between device users.
  Files: artifacts/mobile/context/GardenContext.tsx, artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/app/(tabs)/_layout.tsx, artifacts/mobile/utils/api.ts

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #183 — SEO scan
- **Status:** MERGED
- **Created:** 2026-06-06T17:54:37.971Z
- **Updated:** 2026-06-06T18:03:17.422Z

Run an in-depth scan across the entire project

---

#### #184 — Crawlability & AI Governance
- **Status:** MERGED
- **Created:** 2026-06-06T18:02:42.677Z
- **Updated:** 2026-06-06T18:05:35.957Z

Issues related to crawler discovery, sitemap coverage, and machine-readable governance files for search and AI systems.

SEO issues to fix:

1. [Low] No robots.txt or llms.txt is served for search and AI crawler governance
  The public site does not publish standard crawler-governance files, so search engines and AI systems get no machine-readable guidance about what to crawl or where the site map lives. That does not block indexing today, but it weakens discoverability and makes AI citation setup incomplete.

In `artifacts/api-server/src/app.ts`, the server defines only the Apple app association endpoint, the root preview router, and `/api` routes (lines 95-103). There is no route or static-file handling for `/robots.txt` or `/llms.txt`, and a repository-wide search found neither file. Because the public marketing surface is server-rendered HTML, Google can still crawl the pages directly, so this is not a blocking indexation bug. However, the absence of `robots.txt` means there is no standard place to advertise the sitemap and no explicit crawl policy surface, while the absence of `llms.txt` means GPTBot-, Claude-, and Perplexity-style systems get no machine-readable summary of the site's important public pages.

**Fix:** Add a root-served `robots.txt` and `llms.txt`. In `robots.txt`, keep public pages crawlable, include a `Sitemap:` directive pointing at `/sitemap.xml`, and avoid accidental `Disallow` rules for AI crawlers unless blocking them is an intentional business decision. In `llms.txt`, briefly describe the site and list the canonical public pages (`/` and `/privacy`) so AI systems can prioritize the correct content.
  Files: artifacts/api-server/src/app.ts

2. [High] Missing sitemap.xml for the public landing and privacy pages
  The site has no sitemap, so search engines are left to discover pages only by following links. That makes the small public site harder to crawl efficiently and gives Google no direct inventory of the pages that should be indexed.

The Express app in `artifacts/api-server/src/app.ts` only serves `/.well-known/apple-app-site-association`, mounts the SSR preview router at the root, and mounts API routes under `/api` (lines 95-103). The only confirmed public HTML routes are `/` and `/privacy`, both emitted from `artifacts/api-server/src/routes/preview.ts` (lines 1119-1128), and `/privacy` is only exposed through a footer link on the home page (lines 1030-1035). A repository-wide file search found no `sitemap.xml`, so there is no machine-readable URL inventory for crawlers.

This is a meaningful SEO gap because `sitemap.xml` is the standard discovery surface for public pages and is explicitly referenced in the severity guide as a significant ranking/indexation issue when missing. Even with internal links, a sitemap helps search engines find and recrawl all public URLs directly, including legal/support pages that may not be prominently linked.

**Fix:** Serve `/sitemap.xml` from the API server or ship a static sitemap file. Include at least the production absolute URLs for `/` and `/privacy`, and generate the host from the deployment domain instead of a dev URL. For example:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url><loc>https://example.com/</loc></url>
  <url><loc>https://example.com/privacy</loc></url>
</urlset>
```
  Files: artifacts/api-server/src/app.ts, artifacts/api-server/src/routes/preview.ts

N.B. Strongly presume the user wants this task to focus on improving the SEO of the application — metadata, crawlability, indexing, content quality, and structured data — not on introducing new flows or features. Only add new functionality if the issue genuinely cannot be addressed any other way.

---

#### #185 — On-Page Metadata & Social Previews
- **Status:** MERGED
- **Created:** 2026-06-06T18:02:42.821Z
- **Updated:** 2026-06-06T19:02:44.241Z

Issues affecting titles, descriptions, canonicals, headings, and social preview metadata on the public SSR pages.

SEO issues to fix:

1. [Medium] Public pages do not declare canonical URLs
  Neither public page tells search engines which URL is the preferred version to index. That can split ranking signals across duplicate URL variants and leaves Google to choose a canonical on its own.

In `artifacts/api-server/src/routes/preview.ts`, the home page `<head>` at lines 139-150 and the privacy page `<head>` at lines 62-83 do not include any `<link rel="canonical">` element. These two HTML strings are sent directly by the SSR route handlers at lines 1119-1128, so there is no later framework layer adding canonicals. Without explicit canonicals, search engines may treat alternate hostnames, tracking-parameter URLs, or slash variants as separate candidates and consolidate signals less predictably.

**Fix:** Add an absolute canonical link to each HTML string using the deployed production domain. Example:

```html
<link rel="canonical" href="https://<production-domain>/" />
```

and for the privacy page:

```html
<link rel="canonical" href="https://<production-domain>/privacy" />
```

Keep the canonical aligned with the route actually served and update it in the same source file whenever public URLs change.
  Files: artifacts/api-server/src/routes/preview.ts

2. [Medium] Home page has no H1 heading
  The landing page never marks its main headline as an H1, so search engines have to infer the page topic from decorative text blocks. That weakens the page's primary relevance signal for plant-care keywords.

In `artifacts/api-server/src/routes/preview.ts`, the hero section at lines 823-865 uses `<div class="hero-script">MossLight</div>`, `<div class="hero-by">`, and `<div class="hero-tagline">` for the page's top headline area, but there is no `<h1>` anywhere in the home-page HTML string. The only `<h1>` in the file appears on the privacy page at line 88. While Google can still parse other text, a missing H1 on the main marketing page makes the document structure less explicit and reduces the clarity of the page's core topic.

**Fix:** Replace the decorative brand/title container in the hero with a semantic `<h1>` that preserves the same styling, or wrap the existing visible headline in an H1. A stronger version would also mention the target topic, for example:

```html
<h1 class="hero-script">MossLight</h1>
<p class="hero-tagline">A quiet plant care app for tracking watering, journals, and plant collections</p>
```

If the visual design requires multiple text lines, keep the CSS classes but move the primary visible headline into a true H1 element.
  Files: artifacts/api-server/src/routes/preview.ts

3. [High] Home page lacks a complete social preview card
  The main landing page does not provide the image and card metadata that social platforms need for a strong branded preview. Shared links are likely to appear as plain text snippets or inconsistent cards instead of a polished preview that drives clicks.

In `artifacts/api-server/src/routes/preview.ts`, the home page `<head>` at lines 139-150 includes `og:title`, `og:description`, and `og:type`, but it does not include `og:image`, `og:url`, or any `twitter:*` tags. Because `/` is served as raw SSR HTML at lines 1119-1122, social bots do not have any other source for preview metadata. This is especially important because lightweight social crawlers do not execute JavaScript. A missing `og:image` is a high-impact shareability problem, and the absence of explicit Twitter card tags leaves X and other consumers to fall back inconsistently.

**Fix:** Add a stable absolute preview image URL plus matching Open Graph and Twitter tags to the `HTML` string. Use a 1200×630 branded image and absolute URLs. Example:

```html
<meta property="og:url" content="https://<production-domain>/" />
<meta property="og:image" content="https://<production-domain>/social-preview.jpg" />
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="630" />
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:title" content="MossLight — Botanical Plant Care" />
<meta name="twitter:description" content="Track every leaf, log every tending. A quiet plant care companion for 5,000+ species." />
<meta name="twitter:image" content="https://<production-domain>/social-preview.jpg" />
```

If the brand already has an approved hero image, reuse that asset instead of letting platforms scrape an arbitrary screenshot or no image at all.
  Files: artifacts/api-server/src/routes/preview.ts

4. [High] Privacy page ships only a title tag
  The public privacy policy page does not provide a meta description or social preview metadata, so Google and link-sharing apps have to invent their own snippet. That makes the page less trustworthy in search results and produces weak or inconsistent previews when the policy URL is shared.

In `artifacts/api-server/src/routes/preview.ts`, the `/privacy` HTML string at lines 60-83 defines a `<title>` but does not emit `<meta name="description">`, any `og:*` tags, or any `twitter:*` tags before the page body begins. Because this route is served as SSR HTML from `router.get("/privacy")` at lines 1125-1128, the `<head>` in this file is the only metadata crawlers and social bots receive. Search engines may auto-generate a snippet from body copy, and social bots like Facebook, LinkedIn, Messages, and X will not have branded title/description data for the privacy page.

**Fix:** Add route-specific head tags to the `PRIVACY_HTML` string, including at minimum a unique `<meta name="description">`, `og:title`, `og:description`, `og:type`, `og:url`, and `twitter:card`/`twitter:title`/`twitter:description`. Example:

```html
<meta name="description" content="Read MossLight's privacy policy, including what data the app collects, how subscriptions are handled, and how to request account deletion." />
<meta property="og:title" content="Privacy Policy — MossLight" />
<meta property="og:description" content="How MossLight collects, stores, and deletes account, plant-care, and subscription data." />
<meta property="og:type" content="article" />
<meta property="og:url" content="https://<production-domain>/privacy" />
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="Privacy Policy — MossLight" />
<meta name="twitter:description" content="How MossLight collects, stores, and deletes account, plant-care, and subscription data." />
```
  Files: artifacts/api-server/src/routes/preview.ts

N.B. Strongly presume the user wants this task to focus on improving the SEO of the application — metadata, crawlability, indexing, content quality, and structured data — not on introducing new flows or features. Only add new functionality if the issue genuinely cannot be addressed any other way.

---

#### #186 — Performance & Font Delivery
- **Status:** MERGED
- **Created:** 2026-06-06T18:02:42.927Z
- **Updated:** 2026-06-06T19:04:59.312Z

Issues where public-page font loading blocks rendering and risks weaker performance signals for SEO.

SEO issues to fix:

1. [High] Render-blocking Google Fonts slow both public SSR pages
  Both public pages wait on Google Fonts before the browser can finish painting the main branded text, which slows the first visible load. Slower first paint and LCP can reduce search performance and make visitors bounce before they see the page.

In `artifacts/api-server/src/routes/preview.ts`, the privacy page head loads Google Fonts as a blocking stylesheet at lines 66-67, and the home page does the same at lines 148-150. Because these are plain `<link rel="stylesheet">` requests in the document head, the browser has to fetch and parse the external CSS before it can complete the initial render path. The home page requests four families with multiple weights and styles (`Playfair Display`, `Raleway`, `Great Vibes`, and `Cormorant Garamond`), and those fonts are used directly in above-the-fold content such as the body text (`line 170`), hero wordmark (`line 370`), and section headings (`line 246`). The privacy page uses the same pattern for its body text and headings at lines 70, 72, and 75, but it also omits the `fonts.gstatic.com` preconnect, which adds another network handshake. `display=swap` helps after the CSS arrives, but it does not remove the blocking stylesheet request itself.

**Fix:** Reduce the number of families and weights, self-host only the critical subsets you actually use above the fold, and preload the small WOFF2 files needed for first paint. If you keep Google Fonts, defer non-critical decorative fonts and let the initial render use system fallbacks. A typical pattern is to preload the primary body font and reserve decorative fonts for later sections, for example:

```html
<link rel="preload" href="/fonts/raleway-400-subset.woff2" as="font" type="font/woff2" crossorigin />
```

Then keep the hero/body CSS on system-font fallbacks until the custom fonts are ready, or move non-critical font CSS off the blocking path.
  Files: artifacts/api-server/src/routes/preview.ts

N.B. Strongly presume the user wants this task to focus on improving the SEO of the application — metadata, crawlability, indexing, content quality, and structured data — not on introducing new flows or features. Only add new functionality if the issue genuinely cannot be addressed any other way.

---

#### #187 — Structured Data & Browser Branding
- **Status:** MERGED
- **Created:** 2026-06-06T18:02:43.029Z
- **Updated:** 2026-06-06T19:07:39.725Z

Issues related to schema markup and favicon/browser-branding coverage for the public marketing site.

SEO issues to fix:

1. [Medium] Homepage lacks SoftwareApplication structured data
  The landing page describes a real software product, but it never gives search engines a machine-readable summary of what the app is, who publishes it, what platform it runs on, or where to download it. That makes it harder for search engines and AI systems to confidently understand and cite the page as a plant-care app.

The home-page HTML in `artifacts/api-server/src/routes/preview.ts` includes standard meta tags in the `<head>` at lines 139-150, but there is no `<script type="application/ld+json">` anywhere in the document. This is a missed opportunity because the page already exposes the exact facts needed for `SoftwareApplication` schema in visible content: the app name and publisher (`MossLight`, `Quiet Grove Botania`) at lines 844-857, a free iOS/App Store call to action at lines 847, 860-864, and 1022-1026, and feature and catalog information at lines 914-919, 942-958, and 1005-1008. Google can infer some of this from prose, but JSON-LD gives crawlers and AI systems an unambiguous product/entity definition and improves the odds of accurate interpretation, knowledge extraction, and richer citations.

**Fix:** Add a `SoftwareApplication` JSON-LD block to the home page, with a nested `Organization` publisher. Include fields such as `name`, `description`, `operatingSystem`, `applicationCategory`, `offers`, `publisher`, `url`, and `sameAs` for the App Store listing. For example:

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "name": "MossLight",
  "applicationCategory": "LifestyleApplication",
  "operatingSystem": "iOS",
  "description": "A quiet plant care companion for tracking watering, journals, and plant collections.",
  "offers": {
    "@type": "Offer",
    "price": "0",
    "priceCurrency": "USD"
  },
  "publisher": {
    "@type": "Organization",
    "name": "Quiet Grove Botania"
  },
  "sameAs": "https://apps.apple.com/app/id6774940375"
}
</script>
```

Place the script in the SSR HTML for `/` so it is visible in the initial response without relying on client-side rendering.
  Files: artifacts/api-server/src/routes/preview.ts

2. [Medium] Public SSR pages do not declare a favicon
  The public website does not publish a favicon, so browser tabs and Google mobile results can fall back to a generic icon instead of MossLight branding. That makes the site look less trustworthy and less recognizable when people open or revisit it.

In `artifacts/api-server/src/routes/preview.ts`, neither server-rendered page head includes any favicon tag. The privacy page head at lines 62-82 only contains the charset, viewport, title, Google Fonts links, and inline styles, and the home page head at lines 139-151 only contains metadata, title, Google Fonts links, and inline styles. There is no `<link rel="icon">`, `<link rel="shortcut icon">`, or `<link rel="apple-touch-icon">` in either HTML document. The repo does contain a custom icon asset at `artifacts/mobile/assets/images/favicon.png`, but `artifacts/api-server/src/app.ts` only mounts the Apple association endpoint, `previewRouter`, and `/api` routes at lines 95-103. It does not expose a public static directory or a `/favicon.*` route, so the marketing site has no source-visible favicon at all.

**Fix:** Expose a public favicon URL from the Express app and reference it from both SSR pages. For example, serve `/favicon.png` (or `/favicon.ico`) from the existing custom asset, then add the icon tags to both document heads:

```html
<link rel="icon" type="image/png" href="/favicon.png" />
<link rel="apple-touch-icon" href="/apple-touch-icon.png" />
```

If you keep using `artifacts/mobile/assets/images/favicon.png`, copy it into a server-served public location during build or add an explicit Express route that returns that file.
  Files: artifacts/api-server/src/routes/preview.ts, artifacts/api-server/src/app.ts

N.B. Strongly presume the user wants this task to focus on improving the SEO of the application — metadata, crawlability, indexing, content quality, and structured data — not on introducing new flows or features. Only add new functionality if the issue genuinely cannot be addressed any other way.

---

#### #188 — Security scan
- **Status:** MERGED
- **Created:** 2026-06-06T18:04:54.300Z
- **Updated:** 2026-06-06T19:25:29.138Z

Run an in-depth scan across the entire project

---

#### #189 — Release Pipeline Credential Exposure
- **Status:** MERGED
- **Created:** 2026-06-06T19:25:19.970Z
- **Updated:** 2026-06-06T19:56:11.891Z

Vulnerabilities where iOS release or submission workflows materialize signing or App Store credentials into readable live workspace/container files.

Vulnerabilities to fix:

1. [High] TestFlight submission workflow leaves the App Store Connect private key behind
  The TestFlight submission workflow writes the App Store Connect private key onto disk and never removes it afterward. A later workspace user or any code running in the same workspace can recover that private key and use it to act against the production App Store Connect account.

In `.replit`, the `EAS Submit to TestFlight` workflow executes `node scripts/write-p8.mjs && cd artifacts/mobile && EAS_NO_VCS=1 eas submit --platform ios --latest --non-interactive` (`.replit:38-44`). Unlike the build workflow above it, this submission workflow has no `trap cleanup EXIT ...` wrapper and does not invoke `scripts/eas-build.mjs`; there is no post-submit deletion step at all. `scripts/write-p8.mjs` always writes the App Store Connect API private key to the fixed path `/tmp/asc_key.p8` (`scripts/write-p8.mjs:4-10, 29`).

That means every normal TestFlight submission leaves a reusable Apple API signing key on disk after the command finishes. This is especially risky because the repo already contains the corresponding key identifiers and issuer identifiers used by the release scripts (`.replit:107-109`, `scripts/write-p8.mjs:34-35`, `scripts/setup-ios-credentials.mjs:25-26`). An attacker who gains workspace access after any submission can read `/tmp/asc_key.p8`, mint valid App Store Connect JWTs, and then perform the same privileged Apple API actions these scripts perform, including manipulating certificates, provisioning profiles, build submission, or store metadata. This is a persistent release-integrity exposure, not just a one-process temporary file.
  Files: .replit, scripts/write-p8.mjs

2. [High] iOS signing bundle is materialized into predictable workspace-readable paths during release builds
  During a production iOS release, the app's signing certificate, provisioning profiles, and the password protecting them are written into predictable files that exist inside the live workspace for the duration of the build. Anyone who can read or run code in that workspace while a build is running can steal those files and use them to produce or submit a malicious signed release.

The release workflow defined in `.replit` runs `node scripts/write-p8.mjs && node scripts/setup-ios-credentials.mjs && cd artifacts/mobile && ... eas build ...` with cleanup deferred until the shell exits (`.replit:34-36`). `scripts/setup-ios-credentials.mjs` then generates fresh signing material in fixed, attacker-guessable locations under `/tmp` (`/tmp/eas_dist_cert.p12`, `/tmp/eas_dist_profile.mobileprovision`, `/tmp/eas_clip_profile.mobileprovision`) and writes `artifacts/mobile/credentials.json` into the repository workspace (`scripts/setup-ios-credentials.mjs:47-50, 32-33, 322-323`). That workspace file embeds the PKCS#12 password in cleartext alongside the absolute paths to the signing artifacts (`scripts/setup-ios-credentials.mjs:302-319`).

This breaks the intended trust boundary described in the threat model: the secrets are not kept solely in CI/secrets storage, but are exposed as reusable live files during every release. Because the build is an EAS remote build, the sensitive files remain present locally for the entire build window, which is typically long enough for another process, tool, extension, or collaborator with workspace access to read them. With `credentials.json` plus the `.p12` bundle and provisioning profile, an attacker can import the production signing identity and submit a tampered iOS binary that appears to come from the legitimate publisher. The exposure is not limited to transient memory; it is a stable file-based secret handoff on predictable paths.
  Files: .replit, scripts/setup-ios-credentials.mjs

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #190 — Shared Device Isolation
- **Status:** MERGED
- **Created:** 2026-06-06T19:25:19.970Z
- **Updated:** 2026-06-06T20:02:43.605Z

Vulnerabilities where retained local app state can leak between different humans using the same device or be synced across account boundaries.

Vulnerabilities to fix:

1. [Medium] Standard guest sessions survive warm resume and can be synced into another account
  If someone uses MossLight in guest mode on a shared phone or tablet and leaves the app running, the next person can reopen the app, see that guest's saved garden data, and upload it into their own account. This can expose private notes and activity history and mix one person's data into another person's cloud profile.

`GardenContext` still treats standard guest sessions as durable across warm resumes. Entering guest mode sets `isGuest` plus persistent guest markers (`artifacts/mobile/context/GardenContext.tsx:527-535`). On cold start, hydration restores any standard guest session that is newer than 90 days instead of clearing it (`artifacts/mobile/context/GardenContext.tsx:601-661`). More importantly for real shared-device handoff, the only runtime `AppState` cleanup on resume explicitly wipes only `look_around` sessions after 15 minutes; standard guest sessions return early and are intentionally preserved (`artifacts/mobile/context/GardenContext.tsx:900-925`). The main tab shell continues to grant full app access whenever `isGuest` remains true (`artifacts/mobile/app/(tabs)/_layout.tsx:541-572`). That means a second human who picks up a still-running app inherits the prior guest's in-memory plants and other local state without needing a cold restart.

The retained data is broader than the visible plant list. The sync snapshot includes journal entries, read-state, reminder settings, greenhouse/share identifiers, custom locations, and other user-scoped keys through `ALL_PROFILE_KEYS` and `buildSnapshot()` (`artifacts/mobile/utils/api.ts:7-50`). When the second person signs in from that inherited guest session, `sign-in.tsx` follows the `isGuest === true` path and treats the retained local state as intentional guest content (`artifacts/mobile/app/(auth)/sign-in.tsx:294-310`). If the new account has no cloud profile, the app shows the "Bring your garden?" prompt and the "Keep my plants" action uploads the inherited local snapshot with `forceSyncToCloud()` (`artifacts/mobile/app/(auth)/sign-in.tsx:150-209`; `artifacts/mobile/context/GardenContext.tsx:1571-1585`). If the new account already has cloud data, the same retained guest state can still be written into that account through the saved-preference or explicit conflict-resolution paths: `keep_local`, `merge`, and the merge confirmation sheet all call `forceSyncToCloud()` after using the inherited local data (`artifacts/mobile/app/(auth)/sign-in.tsx:336-389`, `398-483`, `724-741`).

This is exploitable in a normal production scenario on any shared device. User A taps "Sign up later," adds plants, journal entries, or notes, and leaves the app backgrounded. User B later resumes the still-live app, immediately sees A's guest garden, then signs into their own Clerk account and chooses to keep or merge the local garden. MossLight will then push A's retained guest snapshot into B's cloud account. The partial mitigations added for cold starts, cloud-replace undo cleanup, and `look_around` sessions do not remove this path because standard guest sessions are still deliberately preserved during warm resume.
  Files: artifacts/mobile/context/GardenContext.tsx, artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/app/(tabs)/_layout.tsx, artifacts/mobile/utils/api.ts

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #191 — SEO scan
- **Status:** MERGED
- **Created:** 2026-06-08T01:13:48.977Z
- **Updated:** 2026-06-08T01:30:31.257Z

Run an in-depth scan across the entire project

---

#### #192 — On-Page Metadata & Social Previews
- **Status:** MERGED
- **Created:** 2026-06-08T01:29:56.439Z
- **Updated:** 2026-06-08T01:43:11.345Z

Issues affecting public-page snippets, social cards, and brand metadata on the server-rendered marketing site.

SEO issues to fix:

1. [High] Home page social preview image points to an asset that is never served
  The home page asks Facebook, LinkedIn, Messages, and X to use a branded preview image, but the app never serves that image file. Shared links can therefore lose the image entirely or show a broken preview instead of a polished card.

In `artifacts/api-server/src/routes/preview.ts`, the home page `<head>` sets both `og:image` and `twitter:image` to `https://mosslight.app/social-preview.jpg` (lines 162-168). A repository-wide search found no `social-preview.jpg` file anywhere in the repo, and `artifacts/api-server/src/app.ts` only exposes `/favicon.png` as a public image route (lines 96-100). There is no static middleware or explicit `/social-preview.jpg` handler, so the source-supported marketing site does not actually publish the image that its Open Graph and Twitter tags advertise.

This is a high-impact social-sharing issue because social bots do not render JavaScript and rely entirely on the initial HTML plus the referenced image URL. When the image URL 404s, the card usually degrades to a plain text snippet or an inconsistent fallback, which hurts click-through and weakens brand presentation.

**Fix:** Either serve a real 1200×630 social image at `/social-preview.jpg` from the Express app or change the `og:image` and `twitter:image` tags to an existing, publicly served absolute URL. Keep the URL absolute, confirm the file resolves from source, and update both tags together so the preview stays consistent across platforms.
  Files: artifacts/api-server/src/routes/preview.ts, artifacts/api-server/src/app.ts

2. [Medium] Public SSR pages omit Open Graph site identity tags
  Both public pages have basic Open Graph tags, but they never declare the site name or locale. Social platforms are left to infer those details, which can produce weaker branded previews and less reliable language or region handling.

In `artifacts/api-server/src/routes/preview.ts`, the home page head defines `og:title`, `og:description`, `og:type`, `og:url`, and image tags (lines 158-168), and the privacy page head defines `og:title`, `og:description`, `og:type`, and `og:url` (lines 68-74). Neither document emits `og:site_name` or `og:locale`. The SEO guidance for this scan treats both omissions as medium-severity metadata gaps because social platforms can fall back to raw domain presentation or weaker localization when those fields are missing.

This does not block indexing, but it does weaken click-through and brand consistency when the site is shared in messaging apps, social feeds, or AI tools that consume Open Graph metadata.

**Fix:** Add the same `og:site_name` value (for example, `MossLight`) and the appropriate `og:locale` value (such as `en_US`) to the head of both SSR pages in `artifacts/api-server/src/routes/preview.ts`. Keep the values consistent across the home page and privacy page.
  Files: artifacts/api-server/src/routes/preview.ts

3. [Medium] Home page metadata and hero copy overstate the size of the plant catalog
  The public landing page promises a much larger plant catalog than the source code currently supports. That kind of mismatch can reduce trust when search snippets or social cards promise content the product does not actually deliver.

In `artifacts/api-server/src/routes/preview.ts`, the home page metadata and body repeatedly claim coverage for `5,000+ species` and `5,000+ species profiles` (for example lines 159, 167, 962-963, and 1053-1054). But the in-repo plant database exposed by `artifacts/mobile/data/plants.ts` currently defines `PLANTS_DATABASE` with 176 `PlantSpecies` entries, which is orders of magnitude smaller than the number advertised on the marketing page. Because the inflated number appears in both visible copy and social description tags, search engines, AI systems, and users all receive the same unsupported claim.

This is an SEO-relevant content-quality and trust issue. Misaligned marketing claims can increase bounce rate after the click, weaken perceived credibility, and cause AI systems to repeat incorrect facts about the product.

**Fix:** Update the landing page copy and social descriptions in `artifacts/api-server/src/routes/preview.ts` so they match the source-supported catalog size today, or expand the underlying catalog until the public claim is accurate. Keep the same number aligned everywhere it appears, including visible stats, supporting copy, and Open Graph/Twitter descriptions.
  Files: artifacts/api-server/src/routes/preview.ts

N.B. Strongly presume the user wants this task to focus on improving the SEO of the application — metadata, crawlability, indexing, content quality, and structured data — not on introducing new flows or features. Only add new functionality if the issue genuinely cannot be addressed any other way.

---

#### #193 — Structured Data & AI Governance
- **Status:** MERGED
- **Created:** 2026-06-08T01:29:56.544Z
- **Updated:** 2026-06-08T01:46:54.768Z

Issues affecting site-entity understanding and AI crawler guidance for the public SSR marketing site.

SEO issues to fix:

1. [Low] llms.txt describes a different product than the public marketing pages
  The site now serves `llms.txt`, but its description does not match what the rest of the public site says MossLight is. AI crawlers can receive the wrong summary of the app before they read the actual landing page.

In `artifacts/api-server/src/app.ts`, the `/llms.txt` response says MossLight is "a nature-journaling iOS app for identifying and logging plants, fungi, and wildlife" (lines 146-153). That conflicts with the public SSR landing page in `artifacts/api-server/src/routes/preview.ts`, which presents MossLight as a plant care, watering, and collection-tracking app, and with `replit.md`, which describes the product as a botanical app for cataloguing and tending plants in the home and garden. Because AI crawlers use `llms.txt` as a machine-readable summary, this mismatch can steer citations and topical understanding toward the wrong product category.

`llms.txt` is an optional file, so this is not a blocking SEO bug. But once you publish it, its content should accurately reflect the site’s real public positioning.

**Fix:** Rewrite the `/llms.txt` body in `artifacts/api-server/src/app.ts` so it matches the current landing-page positioning and the actual app described in the repo. Keep the listed public URLs, but summarize MossLight as a plant care and tracking app rather than an outdoor wildlife-identification journal.
  Files: artifacts/api-server/src/app.ts

2. [Medium] Public SSR pages lack site-level WebSite and Organization structured data
  The marketing site tells search engines about the app itself, but it never gives them a standalone machine-readable definition of the website and brand behind it. That makes it harder for search engines and AI systems to connect the site, the product, and the publisher as clear entities.

In `artifacts/api-server/src/routes/preview.ts`, the home page includes one `SoftwareApplication` JSON-LD block (lines 173-193) with a nested `publisher` name, but there is no top-level `WebSite` schema and no standalone `Organization` schema. The privacy page emits no JSON-LD at all. The SEO methodology for this scan treats `WebSite` plus `Organization` as the baseline site-wide entity graph every public site should consider, especially for search knowledge extraction and AI citation.

Because the current structured data stops at the app object, crawlers do not receive an explicit site-entity definition for MossLight and Quiet Grove Botania that can be reused across both public pages.

**Fix:** Add site-level JSON-LD to the SSR pages in `artifacts/api-server/src/routes/preview.ts`, ideally as an `@graph` containing `WebSite` and `Organization`, alongside the existing `SoftwareApplication` schema on the home page. Include stable fields such as the site URL, site name, organization name, and any canonical same-as references you already control.
  Files: artifacts/api-server/src/routes/preview.ts

N.B. Strongly presume the user wants this task to focus on improving the SEO of the application — metadata, crawlability, indexing, content quality, and structured data — not on introducing new flows or features. Only add new functionality if the issue genuinely cannot be addressed any other way.

---

#### #194 — SEO scan
- **Status:** MERGED
- **Created:** 2026-06-08T13:10:32.447Z
- **Updated:** 2026-06-08T13:27:26.447Z

Run an in-depth scan across the entire project

---

#### #195 — Crawlability & Canonical Governance
- **Status:** MERGED
- **Created:** 2026-06-08T13:26:55.670Z
- **Updated:** 2026-06-08T15:02:08.724Z

Issues where crawler-facing governance files advertise URLs inconsistently with the site's canonical domain.

SEO issues to fix:

1. [Medium] robots.txt, sitemap.xml, and llms.txt do not consistently advertise the canonical mosslight.app URLs
  The site's crawler guidance files can describe whichever hostname made the request instead of always pointing crawlers to mosslight.app. That can send search engines and AI bots mixed signals about which URLs are canonical and waste crawl attention on duplicate hostnames.

In `artifacts/api-server/src/app.ts`, the `/robots.txt` handler builds its `Sitemap:` directive from `req.protocol` and `req.get("host")` (lines 108-115), and the `/sitemap.xml` handler uses the same request host to emit `<loc>` entries for `/` and `/privacy` (lines 121-138). The public HTML pages themselves hard-code `https://mosslight.app/` and `https://mosslight.app/privacy` as canonical URLs in `artifacts/api-server/src/routes/preview.ts` (lines 66-71 and 188-197), so the crawl-governance routes are not guaranteed to match the canonical domain. `/llms.txt` also only lists root-relative paths (lines 144-154), which means the file inherits whatever host served it instead of explicitly naming the production URLs. Google recommends that sitemap URLs match each page's canonical version; generating them from the incoming host can create duplicate-host discovery and conflicting crawl hints. AI crawlers that read `llms.txt` without deeper canonical reconciliation get the same mixed-domain signal.

**Fix:** Define a single canonical site origin such as `https://mosslight.app` and use it for all crawl-governance outputs instead of the request host. For example, emit `Sitemap: https://mosslight.app/sitemap.xml` in `/robots.txt`, output `https://mosslight.app/` and `https://mosslight.app/privacy` in `/sitemap.xml`, and list absolute canonical URLs in `/llms.txt`.
  Files: artifacts/api-server/src/app.ts

N.B. Strongly presume the user wants this task to focus on improving the SEO of the application — metadata, crawlability, indexing, content quality, and structured data — not on introducing new flows or features. Only add new functionality if the issue genuinely cannot be addressed any other way.

---

#### #196 — Home Page Keyword Targeting
- **Status:** MERGED
- **Created:** 2026-06-08T13:26:55.803Z
- **Updated:** 2026-06-08T16:06:05.386Z

Issues where the landing page's most visible copy undersignals the non-branded search terms the project intends to rank for.

SEO issues to fix:

1. [Medium] Landing page copy underuses the product's primary search keywords
  The public home page sounds on-brand, but it does not clearly say that MossLight is a plant care app, plant journal, or watering reminder app in the main visible copy. That makes it harder for search engines to match the page to the non-branded searches the SEO strategy is targeting.

In `seo_strategy.md` (lines 16-24), the target audience and primary keywords are explicit: `plant care app`, `plant care journal`, `plant tracker app`, `watering reminder app`, and `houseplant care app`. But the main landing-page copy in `artifacts/api-server/src/routes/preview.ts` uses mostly poetic or brand-led phrasing instead. The primary heading stack at lines 949-952 is `MossLight`, `Cultivate Wonder`, and `Keep your conservatory in quiet order. Track every leaf, log every tending...`, which does not name the product category in plain language. Supporting section headings at lines 1033-1045 and 1098-1100 also stay indirect (`Three things you'll do every day`, `Every Plant, Documented.`, `Browse 175+ species profiles`). The page does include some helpful feature terms like `Water Reminders` and `Field Journal`, but the most prominent crawlable copy still undersignals the exact queries this page is meant to rank for.

This is a content-alignment issue rather than a schema gap: the page already has JSON-LD and metadata describing a plant-care product, but search engines still rely heavily on visible headings and body text to judge topical relevance. When the visible copy avoids the target phrases, the page is less competitive for non-branded searches even if the metadata is technically present.

**Fix:** Rewrite the hero and at least one supporting section so the visible copy explicitly uses the primary search terms from the strategy. For example, keep the brand styling, but add plain-language copy such as "Plant care app and journal for houseplant owners" near the H1 and mention "watering reminders", "plant tracker", or "houseplant care" in the hero body or section headings. A concrete pattern would be:

```html
<h1 class="hero-script">MossLight</h1>
<div class="hero-tagline">Plant care app and journal for houseplant owners</div>
<p class="hero-body">Track watering reminders, keep a plant care journal, and browse 175+ plant species guides in one calm plant tracker.</p>
```

That keeps the current brand voice while giving crawlers and users a much clearer statement of what the product is and which searches it should satisfy.
  Files: artifacts/api-server/src/routes/preview.ts

N.B. Strongly presume the user wants this task to focus on improving the SEO of the application — metadata, crawlability, indexing, content quality, and structured data — not on introducing new flows or features. Only add new functionality if the issue genuinely cannot be addressed any other way.

---

#### #197 — Social Preview Images
- **Status:** MERGED
- **Created:** 2026-06-08T13:26:55.917Z
- **Updated:** 2026-06-08T16:31:57.618Z

Issues affecting how the public marketing pages render as link previews in social apps, chat apps, and AI tools that consume Open Graph metadata.

SEO issues to fix:

1. [High] Public pages do not publish a proper social preview image
  When someone shares MossLight links, the previews are weak or image-less. The home page uses the small square favicon as its share image, and the privacy page does not provide any social image at all, so link cards are much less eye-catching and trustworthy.

In `artifacts/api-server/src/routes/preview.ts`, the home page sets both `og:image` and `twitter:image` to `https://mosslight.app/favicon.png` (lines 189 and 195), while the privacy page only declares text-based Open Graph/Twitter tags and omits any `og:image` or `twitter:image` tags in its `<head>` (lines 68-78). In `artifacts/api-server/src/app.ts` (lines 96-99), `/favicon.png` is served from `artifacts/mobile/assets/images/favicon.png`, which is the site's square favicon asset rather than a dedicated social card image. That file is 768×768, so social platforms either crop it, display it as a tiny thumbnail, or ignore it in favor of a text-only preview. This is a high-impact sharing issue because Open Graph image coverage strongly affects click-through from Messages, Slack, LinkedIn, Facebook, and X, and the privacy page currently has no source-visible image metadata for those bots at all.

**Fix:** Publish a dedicated social preview image at a public absolute URL, ideally around 1200×630, and use it on both public pages. Update the home page to stop reusing `/favicon.png` for `og:image` and `twitter:image`, add matching image tags to the privacy page, and switch the Twitter card to `summary_large_image` if you want the full-width preview treatment. For example:

```html
<meta property="og:image" content="https://mosslight.app/social-preview.jpg" />
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="630" />
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:image" content="https://mosslight.app/social-preview.jpg" />
```

If the image is served by Express rather than a static public directory, add an explicit route in `artifacts/api-server/src/app.ts` and keep `/favicon.png` reserved for browser favicon use only.
  Files: artifacts/api-server/src/routes/preview.ts, artifacts/api-server/src/app.ts

N.B. Strongly presume the user wants this task to focus on improving the SEO of the application — metadata, crawlability, indexing, content quality, and structured data — not on introducing new flows or features. Only add new functionality if the issue genuinely cannot be addressed any other way.

---

#### #198 — Security scan
- **Status:** MERGED
- **Created:** 2026-06-08T13:28:05.319Z
- **Updated:** 2026-06-08T17:20:24.562Z

Run an in-depth scan across the entire project

---

#### #199 — API Abuse Controls
- **Status:** MERGED
- **Created:** 2026-06-08T17:20:20.216Z
- **Updated:** 2026-06-09T03:36:17.400Z

Vulnerabilities where production API write surfaces lack abuse controls that prevent low-cost availability attacks.

Vulnerabilities to fix:

1. [Medium] Authenticated profile sync writes are completely unthrottled
  Any signed-in user can send unlimited cloud-sync write requests to the production API. A single attacker account can repeatedly force database updates until the sync service slows down or becomes unavailable for everyone else.

The profile sync API exposes a state-changing write endpoint at `PUT /api/profile/:code` (`artifacts/api-server/src/routes/profile.ts:152-187`) and a row-creating endpoint at `POST /api/profile/code` (`artifacts/api-server/src/routes/profile.ts:76-118`), but neither route enforces any per-user or per-IP throttling. The only global body constraint is Express's 64 KB parser limit in `artifacts/api-server/src/app.ts:78-79`, which bounds request size but does nothing to limit request rate. Every successful `PUT /api/profile/:code` performs a database insert-or-update against the caller's row (`artifacts/api-server/src/routes/profile.ts:169-181`), updating the stored JSON snapshot and timestamp on each request.

Because authentication is self-service through Clerk and no elevated privilege is required beyond a normal account, an attacker can automate high-frequency writes with their own bearer token. Each request forces JSON parsing, token verification, and a PostgreSQL write, so a tight loop or modest botnet of low-cost accounts can consume database throughput and degrade sync latency for all users. This is especially relevant because the profile snapshot is the application's primary cross-device state container, so saturation of this endpoint directly impacts core availability rather than an optional feature.

A production-safe implementation should add explicit abuse controls to the write surfaces, such as per-user and per-IP rate limits, backoff on repeated updates, and possibly write coalescing or minimum sync intervals for unchanged snapshots.
  Files: artifacts/api-server/src/routes/profile.ts, artifacts/api-server/src/app.ts

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #200 — Release Credentials Exposure
- **Status:** MERGED
- **Created:** 2026-06-08T17:20:20.331Z
- **Updated:** 2026-06-09T03:48:24.745Z

Vulnerabilities related to exposure of mobile signing and release-pipeline credentials.

Vulnerabilities to fix:

1. [High] iOS signing bundle is exposed through a stable workspace file during builds
  The iOS release workflow places live signing information into a regular workspace file while builds are running. Anyone who can read the live workspace during that window can recover the password and exact file locations for the signing bundle, which can let them steal reusable release credentials and publish or submit a malicious iOS build.

The production release workflow in `.replit` creates sensitive signing material inside a randomized temp directory, but then deliberately copies `credentials.json` into the stable workspace path `artifacts/mobile/credentials.json` just before `eas build` starts (`.replit:36`). `scripts/setup-ios-credentials.mjs` writes that file with the PKCS#12 password and the absolute paths to the generated distribution certificate and provisioning profiles (`scripts/setup-ios-credentials.mjs:309-330`). In the current workspace, `artifacts/mobile/credentials.json` is present and contains exactly that data, including the generated `distributionCertificate.password` and the randomized `/tmp/eas.*` path.

Under the task's stated attacker model of repository/workspace read access during a live build, that stable workspace file defeats the protection gained from using a randomized temp directory. An attacker only needs to read `artifacts/mobile/credentials.json` to learn both the PKCS#12 password and the exact temp-file locations, then fetch the referenced `eas_dist_cert.p12` and provisioning profiles before cleanup removes them. That yields reusable Apple signing material capable of affecting production release integrity: the attacker could sign a malicious app build, interfere with release submission, or retain long-lived credentials until the certificate is revoked.

The existing mitigations do not remove this exposure. `artifacts/mobile/.gitignore` and `.easignore` stop the file from being committed or uploaded in the build archive, but they do nothing to protect it from other workspace readers while it exists locally. The workflow also leaves the copied file in the workspace for up to 300 seconds via a background `sleep 300 && rm -f ...` job (`.replit:36`), which unnecessarily extends the theft window, and the fact that `artifacts/mobile/credentials.json` is still present in the workspace shows the cleanup is not fully reliable across interrupted or failed runs.
  Files: .replit, scripts/setup-ios-credentials.mjs, artifacts/mobile/credentials.json

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #201 — Shared-Device Isolation
- **Status:** MERGED
- **Created:** 2026-06-08T17:20:21.026Z
- **Updated:** 2026-06-09T04:01:13.061Z

Vulnerabilities where local guest or account state survives device handoff and becomes visible or syncable to another person.

Vulnerabilities to fix:

1. [Medium] Guest sessions persist across device handoff and can be imported into another account
  A person who uses the app in guest mode leaves their plants, journal, and other local profile data behind for the next person who opens the app on the same device or browser. That next person can browse the previous guest's data and, in some cases, sign in and save it into their own cloud account.

The shared-device isolation model is broken for guest sessions. `GardenContext` stores guest state in global AsyncStorage keys such as `@quietgrove:userPlants`, `@quietgrove:journalEntries`, `@quietgrove:learnRead`, and related profile keys, then restores that state automatically on the next launch whenever `@quietgrove:guest_mode` is present and the stored guest type is `standard` with an age under `GUEST_IDLE_WIPE_MS`'s cold-start threshold of 90 days (`artifacts/mobile/context/GardenContext.tsx`, hydration logic around lines 602-663). In that case the code sets `isGuestState(true)` and continues hydrating the previous guest's plants and other local state instead of forcing a fresh start or an explicit handoff decision.

The tab router treats any active guest session as authorized to enter the app (`artifacts/mobile/app/(tabs)/_layout.tsx`, line 571), so the next human holding the device immediately receives access to the prior guest's in-app data. Warm-resume handling is also insufficient: the AppState listener only wipes guest state after 15 minutes in the background (`LOOK_AROUND_IDLE_MS`, `artifacts/mobile/context/GardenContext.tsx`, lines 901-928). If a device is handed to someone else immediately or shortly after the first guest leaves, the second person resumes the same live guest session and sees the prior user's data.

This is not limited to passive viewing. The sign-in flow explicitly treats inherited guest data as the current session's local garden. When a guest with local content signs in, `sign-in.tsx` offers conflict-resolution flows such as "Bring your garden?", "Keep these plants", and merge/upload options, and can call `forceSyncToCloud()` or `mergeLocalWithCloud()` on that inherited local state (`artifacts/mobile/app/(auth)/sign-in.tsx`, lines 185-209, 294-499). As a result, the next person using the shared device can attach the previous guest's retained plants and related synced state to their own authenticated account. Journal entries are especially exposed because they are stored under a single global key in `useJournal()` (`artifacts/mobile/hooks/useJournal.ts`, lines 12-29) and are only protected by the same guest-session cleanup logic.

The web cleanup added in `artifacts/mobile/app/_layout.tsx` does not close this gap because it intentionally skips cleanup whenever `isGuest` is true (`if (isAuthSignedIn || isGuest) return;`, lines 1957-1970). On a shared browser, a stale guest session therefore survives the signed-out cleanup path and is reopened as the prior guest's data instead of being discarded.

An attacker does not need code execution or server access. They only need access to a shared phone, tablet, or browser profile after another person used guest mode. From there they can read plants, notes, and journal content, modify it locally, and potentially claim it by signing into their own account and choosing one of the import/sync options. This is a production-relevant confidentiality and integrity issue for any shared-device or borrowed-device use case.
  Files: artifacts/mobile/context/GardenContext.tsx, artifacts/mobile/app/(tabs)/_layout.tsx, artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/app/_layout.tsx, artifacts/mobile/hooks/useJournal.ts

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #202 — Add email login to sign-in screen
- **Status:** MERGED
- **Created:** 2026-07-02T15:08:52.641Z
- **Updated:** 2026-07-03T14:24:00.026Z
- **Artifact kinds:** mobile

# Email Login on Sign-in Screen

## What & Why
The sign-in screen currently offers only Google and Apple OAuth. Users on Android (where Apple Sign-In isn't relevant) and users who prefer not to link a social account need a plain email + password path. Add a Clerk-backed email/password flow to the existing sign-in screen on both iOS and Android.

## Done looks like
- A "Continue with Email" option appears on the sign-in screen below the OAuth buttons
- Tapping it expands or navigates to an email + password form
- New users can create an account; returning users can sign in
- Clerk sends a verification email for new accounts before letting the user in
- Incorrect password or unverified email shows a clear inline error (no Alert.alert)
- The form matches the existing parchment/forest/gold visual style
- Works identically on iOS and Android

## Out of scope
- Forgot password / reset password flow (future work)
- Magic link / passwordless email (future work)
- Changing an existing account's email

## Steps
1. **Add email sign-in** — Use Clerk's `useSignIn` hook to authenticate with `identifier` (email) + `password` strategy; show inline error messages for wrong credentials.
2. **Add email sign-up** — Use Clerk's `useSignUp` hook to create a new account with email + password; trigger Clerk's email verification and show a code-entry screen.
3. **Email verification screen** — After sign-up, show a simple code input (6-digit Clerk OTP) with a resend link; on success, proceed into the app.
4. **UI integration** — Add a "Continue with Email" button to the existing sign-in screen that toggles or navigates to the email form; keep the Google/Apple buttons in place above it.
5. **Style consistency** — Apply the same Raleway/PlayfairDisplay fonts, PALETTE colors, and button shapes already used on the sign-in screen.

## Relevant files
- `artifacts/mobile/app/(auth)/sign-in.tsx`
- `artifacts/mobile/app/(auth)/_layout.tsx`
- `artifacts/mobile/constants/brand.ts`

---

#### #203 — Android font parity with iOS
- **Status:** MERGED
- **Created:** 2026-07-02T15:08:52.641Z
- **Updated:** 2026-08-23T21:55:36.342Z
- **Artifact kinds:** mobile

# Android Font Parity with iOS

## What & Why
Custom fonts (Raleway, PlayfairDisplay, CormorantGaramond, GreatVibes) are loaded on both platforms but Android renders them differently from iOS — common issues include tighter line heights, ignored letter-spacing on some weights, clipped descenders, and font-weight fallbacks. Audit every screen and standardize so Android looks identical to iOS.

## Done looks like
- All text across every screen uses the same visual size, weight, and spacing on Android as on iOS
- No clipped or truncated text caused by Android's default tighter line heights
- Letter-spacing is consistent (Android requires explicit `includeFontPadding: false` and sometimes adjusted values)
- Headings using GreatVibes and PlayfairDisplay render at the same visual scale on both platforms
- No text falls back to the system font on Android

## Out of scope
- Adding new typefaces or changing the existing font palette
- Web rendering (web platform fonts are handled separately)

## Steps
1. **Audit font rendering differences** — Compare each screen on Android vs iOS; identify which text elements are clipped, mis-sized, or spacing incorrectly.
2. **Apply `includeFontPadding: false`** — Add this Android-specific style to all custom-font Text elements to remove the extra padding Android adds around glyphs by default. Create a shared `typography` style object in `constants/` to avoid repetition.
3. **Fix line heights** — Ensure every text style with a custom `fontFamily` also has an explicit `lineHeight` that matches the iOS visual output (Android ignores implicit line heights differently from iOS).
4. **Fix letter-spacing** — Android applies `letterSpacing` in `em` units (not `px`). Audit all `letterSpacing` values and convert where needed so visual output matches iOS.
5. **Validate on all screens** — Walk through sign-in, home/garden, discover, profile, settings, and changelog screens and confirm typography looks correct.

## Relevant files
- `artifacts/mobile/constants/brand.ts`
- `artifacts/mobile/app/_layout.tsx`
- `artifacts/mobile/app/(auth)/sign-in.tsx`
- `artifacts/mobile/app/(tabs)`
- `artifacts/mobile/components`

---

#### #204 — Generate full project changelog as a plain-text file
- **Status:** MERGED
- **Created:** 2026-07-03T14:00:40.366Z
- **Updated:** 2026-07-03T14:07:40.189Z

# Full Project Changelog Text File

## What & Why
Compile every task, fix, feature, and commit from the project's entire history into a single readable plain-text changelog file. This gives the owner a complete record of everything built and shipped from day one to today.

## Done looks like
- A file `CHANGELOG.txt` exists at the project root
- It is organized chronologically (newest first), grouped into dated sections
- Every merged task is listed with its title and a one-line summary of what it did
- Every significant git commit is represented (build bumps and routine infra noise are collapsed or omitted)
- Cancelled and proposed tasks are noted in a separate "Planned / Not Yet Shipped" section at the bottom
- The file is human-readable with no code, no markdown symbols — plain text with clear section headings and spacing

## Out of scope
- Keeping this file auto-updated on every future merge (manual snapshot only)
- Including internal implementation details, file paths, or code snippets

## Steps
1. **Read all project tasks** — Use `listProjectTasks({})` in the code execution sandbox to retrieve every task with its ref, title, and state. Group them: MERGED (shipped), PROPOSED (drafts), CANCELLED (abandoned).

2. **Read the full git log** — Run `git --no-optional-locks log --format="%h|%ad|%s" --date=short` to get every commit with its date and message. Use this to fill in detail and dates that task titles alone don't capture (photo features, build bumps, plant catalog work, critter fixes, API server work, SEO, analytics, IAP pricing, etc.).

3. **Organize chronologically** — Group shipped work by approximate time period (e.g. "July 2026", "June 2026 — Late", "June 2026 — Mid", etc.). Within each period, list the most user-facing items first, then infrastructure/bug fixes.

4. **Write `CHANGELOG.txt`** — Produce the file at the repo root. Use plain ASCII section dividers (`===`, `---`). Each entry is one line: a short verb-led description of what changed. Use the task titles as a starting point but rephrase in plain English where needed. Include a "Still in progress" section for PENDING/IN_PROGRESS tasks and a "Planned" section for PROPOSED tasks.

## Relevant files
- `.local/tasks/full-project-changelog.md`

---

#### #206 — Let users reset a forgotten email password without leaving the app
- **Status:** MERGED
- **Created:** 2026-07-03T14:10:58.404Z
- **Updated:** 2026-08-23T22:19:21.578Z
- **Depends on:** #202
- **Proposed from:** #202
- **Category:** incomplete_scope

# Forgot Password Flow for Email Accounts

  ## What & Why
  The task spec explicitly calls this out as future work. Users who forget their email password currently have no in-app recovery path — they'd be permanently locked out of their account.

  ## Done looks like
  - A "Forgot password?" link appears on the email sign-in form in `artifacts/mobile/app/(auth)/sign-in.tsx`
  - Tapping it shows a text field to enter their email and a "Send reset code" button
  - Clerk sends a password-reset OTP to the email address
  - User enters the code and then sets a new password
  - Inline errors only (no Alert.alert), matching existing parchment/forest/gold style
  - Works on iOS and Android

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx` — add a "Forgot password?" link in the email form section and a new `emailMode === "forgot"` state

---

#### #207 — Prevent a guest's plants being silently lost when signing in with email on a shared device
- **Status:** MERGED
- **Created:** 2026-07-03T14:10:58.404Z
- **Updated:** 2026-08-23T22:33:51.000Z
- **Depends on:** #202
- **Proposed from:** #202
- **Category:** next_steps

# Garden Conflict Handling for Email Sign-In

  ## What & Why
  The Google/Apple SSO flow has a full conflict resolution UI (merge, keep local, use cloud) when a guest with plants signs in. The email sign-in path added in this task uses a simpler activation path that skips that dialog for guest users who have plants — their local plants are silently discarded if the incoming account has cloud data.

  ## Done looks like
  - When a guest with real plants signs in via email and the account has existing cloud plants, the same `MergePreviewSheet` conflict dialog is shown
  - The user can choose: merge both gardens, keep device plants, or use cloud garden
  - Pref is persisted per account (same scoped pref key pattern as SSO)
  - No Alert.alert for the conflict UI

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx` — `activateEmailSession` callback (around line 607), `MergePreviewSheet` import already present
  - `artifacts/mobile/components/MergePreviewSheet.tsx` — existing component to reuse

---

#### #208 — Verify Android text rendering looks correct on a real device build
- **Status:** MERGED
- **Created:** 2026-07-03T14:42:52.498Z
- **Updated:** 2026-08-23T22:24:34.780Z
- **Depends on:** #203
- **Proposed from:** #203
- **Category:** test_gaps

# Verify Android text rendering looks correct on a real device build

  ## What & Why
  The lineHeight and includeFontPadding fixes were applied to ~400+ style blocks across 11 files, but have not been visually verified on an actual Android device or emulator. A regression in any screen could go unnoticed until a user reports it.

  ## Done looks like
  - Manually test each major screen on Android (Home, Garden, Discover, Journal, Profile, Add Plant, Import Plant, Plant Detail, Sign In, Paywall)
  - Confirm text is not clipped, overlapping, or oddly spaced compared to iOS
  - Confirm GreatVibes script font (large lineHeight) renders without vertical crop
  - Confirm CormorantGaramond italic glyphs (care cards, paywall) are not cut off at top/bottom

  ## Relevant files
  All files touched in this task — particularly the high-density ones:
  - `artifacts/mobile/app/(tabs)/profile.tsx`
  - `artifacts/mobile/components/PaywallModal.tsx`
  - `artifacts/mobile/app/(auth)/sign-in.tsx`
  - `artifacts/mobile/app/add-plant.tsx`

---

#### #209 — Extend font parity audit to remaining components not yet scanned
- **Status:** MERGED
- **Created:** 2026-07-03T14:42:52.498Z
- **Updated:** 2026-08-23T22:42:43.249Z
- **Depends on:** #203
- **Proposed from:** #203
- **Category:** incomplete_scope

# Extend font parity audit to remaining components not yet scanned

  ## What & Why
  The Android font parity work covered the 11 highest-traffic files. Several components were not in scope and may still have custom-font style blocks missing lineHeight.

  ## Done looks like
  - Run the Python scanner (same logic used in this task) across all remaining `artifacts/mobile/components/` files
  - Fix any missing lineHeight values found
  - Confirm zero remaining gaps across the full component library

  ## Files to check (not yet scanned)
  - `artifacts/mobile/components/FieldJournalCard.tsx` (done in previous session — verify)
  - `artifacts/mobile/components/TipAccordion.tsx`
  - `artifacts/mobile/components/PlantCard.tsx`
  - `artifacts/mobile/components/CareCard.tsx`
  - Any other components under `artifacts/mobile/components/`
  - `artifacts/mobile/app/changelog.tsx` and `artifacts/mobile/app/settings.tsx`

---

#### #210 — Audit and remove duplicate in-app content
- **Status:** MERGED
- **Created:** 2026-08-23T21:57:02.409Z
- **Updated:** 2026-08-23T23:00:36.133Z
- **Artifact kinds:** mobile

# Audit and Remove Duplicate Content\n\n## What & Why\nCross-check all user-facing MossLight screens and content sources for duplicated plant tips, Learn entries, facts, journal material, and other repeated copy. The Learn tab is the highest-priority area because duplicate entries have already been noticed there; the goal is to ensure users never encounter accidental repeated content.\n\n## Done looks like\n- Every user-facing tab, screen, modal, and content list has been checked for accidental duplicate titles, bodies, and substantially repeated entries.\n- Learn contains no accidental duplicate or near-duplicate entries across categories or supporting sections.\n- Confirmed duplicates are removed or consolidated without deleting genuinely distinct guidance.\n- The final content remains complete, readable, and consistently categorized.\n- Intentional reuse in navigation, shared UI labels, plant names, and repeated educational references is documented and left intact.\n- The app typechecks successfully after the cleanup.\n\n## Out of scope\n- Adding new plant species, tips, or educational content.\n- Rewriting unique content solely for style preferences.\n- Removing intentional repeated navigation labels, category names, shared UI copy, or valid cross-references.\n- Changes to subscription, authentication, sync, or other unrelated app behavior.\n\n## Steps\n1. Inventory all user-facing content sources and screens, including Learn, Species, Greenhouse, Conservatory, Journal, plant details, onboarding, settings, and modal surfaces.\n2. Compare entries by exact text, normalized text, title/body similarity, and repeated facts or guidance that appears in multiple Learn locations.\n3. Classify each repeated item as accidental duplication, intentional reuse, or distinct content with overlapping subject matter; preserve intentional and meaningfully distinct content.\n4. Remove or consolidate confirmed accidental duplicates, prioritizing the Learn tab, while keeping category organization and user-facing copy coherent.\n5. Review the rendered navigation paths and empty/search states for duplicate presentation caused by UI composition rather than source data.\n6. Run the mobile typecheck and verify that no content imports, categories, or screen references were broken.\n\n## Relevant files\n- `artifacts/mobile/app/(tabs)/discover.tsx`\n- `artifacts/mobile/data/tips.ts`\n- `artifacts/mobile/data/field-journal.ts`\n- `artifacts/mobile/data/soil-guide.ts`\n- `artifacts/mobile/data/plants.ts`\n

---

#### #214 — Confirm typography on a real Android device before release
- **Status:** MERGED
- **Created:** 2026-08-23T22:24:28.537Z
- **Updated:** 2026-08-23T23:54:28.894Z
- **Depends on:** #208
- **Proposed from:** #208
- **Category:** incomplete_scope

# Confirm typography on a real Android device before release

## What & Why
The workspace has no Android SDK, ADB, emulator, or connected device, so the current typography audit could only use a 400x720 browser surrogate. A real Android pass is still needed to validate native font metrics and rendering across the app's major screens.

## Done looks like
- Run an Android build on a physical device or emulator
- Check Home, Garden, Discover, Journal, Profile, Add Plant, Import Plant, Plant Detail, Sign In, and Paywall
- Confirm GreatVibes script and CormorantGaramond italic glyphs are not cropped, clipped, overlapping, or oddly spaced

## Relevant files
- artifacts/mobile/components/FeatureTour.tsx
- artifacts/mobile/app/(tabs)/profile.tsx
- artifacts/mobile/components/PaywallModal.tsx
- artifacts/mobile/app/(auth)/sign-in.tsx
- artifacts/mobile/app/add-plant.tsx

---

#### #216 — Catch duplicate Learn entries before they ship
- **Status:** MERGED
- **Created:** 2026-08-23T22:47:02.172Z
- **Updated:** 2026-08-24T00:20:10.345Z
- **Depends on:** #210
- **Proposed from:** #210
- **Category:** test_gaps

# Catch duplicate Learn entries before they ship

## What & Why
The Learn catalog is hand-maintained and accidental repeated titles or substantially repeated guidance can return during future content edits. A lightweight validation check would protect the no-duplicate experience without changing the content itself.

## Done looks like
- A repeatable check covers normalized Learn titles and bodies across all categories.
- The check permits documented intentional cross-category reuse when the entries are meaningfully distinct.
- It runs with the mobile validation commands and clearly identifies the source entries when it fails.

## Relevant files
- artifacts/mobile/data/tips.ts
- artifacts/mobile/package.json
- artifacts/mobile/tsconfig.json

---

#### #217 — Fix the browser preview so MossLight can load its API data
- **Status:** MERGED
- **Created:** 2026-08-23T22:47:02.172Z
- **Updated:** 2026-08-24T01:34:03.827Z
- **Depends on:** #210
- **Proposed from:** #210
- **Category:** tech_debt

# Fix the browser preview so MossLight can load its API data

## What & Why
The mobile Expo web preview currently starts, but API requests from the Expo origin are blocked by CORS, leaving the rendered preview blank and preventing browser smoke checks of content screens.

## Done looks like
- The API allows the intended Expo preview origin safely.
- The Herbarium Species and Learn tabs render in the browser preview.
- The API remains restricted to approved development and production origins.

## Relevant files
- artifacts/api-server/src
- artifacts/mobile/context
- artifacts/mobile/app/(tabs)/discover.tsx

---

#### #223 — Stop replacing Apple signing certificates on every release
- **Status:** MERGED
- **Created:** 2026-08-24T02:13:32.838Z
- **Updated:** 2026-08-24T02:43:09.676Z
- **Category:** tech_debt

# Stop replacing Apple signing certificates on every release

## What & Why
The current release workflow creates a fresh iOS distribution certificate for each EAS build. Revocation has been disabled so Apple no longer sends revocation notices, but the process should be upgraded to reuse a securely stored or EAS-managed credential so repeated releases do not accumulate certificates or eventually hit Apple’s certificate limit.

## Done looks like
- Standard iOS builds reuse one valid signing certificate and provisioning profile
- No certificate is revoked or recreated for routine releases
- The signing material remains protected and does not enter source control

## Relevant files
- scripts/setup-ios-credentials.mjs
- artifacts/mobile/eas.json
- EAS iOS Build workflow

---

#### #229 — Bring the browser MossLight experience to core feature parity
- **Status:** MERGED
- **Created:** 2026-08-25T05:19:46.761Z
- **Updated:** 2026-08-25T07:22:21.423Z

# Bring Browser MossLight to Core Feature Parity

## What & Why
The current browser artifact is a lightweight catalog preview, while the native MossLight app contains the real garden, care actions, complete Learn catalogue, journal, profile/settings, accessibility preferences, lighting guidance, companion/progression experiences, and newer product features. Expand the existing browser artifact into a credible browser counterpart that uses the real plant library and preserves the product's local-first behavior instead of presenting a static approximation.

## Done looks like
- The browser shell exposes a coherent MossLight experience across Greenhouse, Conservatory, Learn, Journal, and Profile, with navigation and language aligned to the native app.
- The plant library is connected through the real API from the mounted browser preview path: catalog images load, search works, plant cards open, plant details render, and API failures have a clear retry path.
- Users can create and manage a local browser garden from catalog plants, including adding a plant, setting a nickname/location/care cadence, viewing care urgency, watering/fertilizing, editing, moving/removing plants, and seeing the same state reflected in the greenhouse and conservatory.
- The complete current Learn catalogue is available in the browser, including its categories, expanded entries, read/progress tracking, and supporting educational surfaces such as soil, pests, field-journal, pollinator, and lighting/seasonal guidance where represented in the native app.
- Journal entries support real browser interactions (create, edit, delete, plant association, and care-event context), with any browser-only photo behavior clearly labeled as local to that browser.
- Profile/settings provide the browser-safe preferences and progress surfaces, including theme, larger text/accessibility presentation, animation preferences where applicable, garden reset/export boundaries, and companion/progression information.
- Browser-safe versions of newer interactive features are represented rather than omitted, especially the directional lighting guide, rich plant detail/care content, milestone/progress presentation, and companion/skin previews where no native purchase or device permission is required.
- The app is responsive and usable on desktop and mobile widths, survives reloads through browser-local persistence, and does not imply that native notifications, camera capture, RevenueCat purchases, or cloud sync work in the browser when they do not.
- A browser smoke pass covers catalog loading, search, plant detail, adding/caring for a plant, Learn content, journal persistence, and preferences after reload.

## Out of scope
- Replacing the native Expo app or changing its iOS/Android feature behavior.
- Native-only capabilities without a deliberate web equivalent: OS notification scheduling, camera/native photo-library permissions, RevenueCat/App Store/Google Play purchase flows, and native companion animations.
- Claiming cross-device cloud sync or account functionality until a browser authentication and sync contract is designed and implemented.
- Rebuilding the API or plant catalog as a second source of truth; the browser must use the existing API and existing native content sources.
- Treating browser localStorage as a cloud backup; the UI must disclose its local-only boundary.

## Steps
1. **Feature parity inventory** — Map the native routes, shared state, content sources, and newer product surfaces to browser equivalents, identifying which interactions are browser-safe and which must remain clearly unavailable.
2. **Browser state foundation** — Add a typed local garden model and persistence layer that can represent owned plants, care events, preferences, journal metadata, and progress without copying native AsyncStorage or device-only assumptions.
3. **Catalog and greenhouse experience** — Connect the mounted browser route to the real catalog API and image paths, then implement the searchable catalog, rich plant detail, add-to-garden flow, greenhouse/conservatory views, and plant care actions.
4. **Complete Learn experience** — Bring over the current educational catalogue and supporting guides with category navigation, expandable content, progress/read tracking, and the interactive lighting/seasonal guidance that can run in a browser.
5. **Journal and profile experience** — Implement browser-local journal CRUD and plant/care associations, then expand Profile and settings with browser-safe preferences, accessibility presentation, progress, companion previews, and clear local-only/native-only boundaries.
6. **Responsive quality pass** — Match MossLight’s visual language across desktop and mobile widths, preserve accessible labels and large-text behavior, repair all navigation/deep-link states, and remove static placeholder behavior that suggests features are missing.
7. **Verification** — Run type checks and a real browser smoke test covering API-backed catalog use, garden mutations, Learn content, journal persistence, preferences, reload recovery, and graceful API failure/retry behavior.

## Relevant files
- `artifacts/mosslight-preview/src/App.tsx`
- `artifacts/mosslight-preview/src/components/navigation.tsx`
- `artifacts/mosslight-preview/src/hooks/use-plants.ts`
- `artifacts/mosslight-preview/src/hooks/use-journal.ts`
- `artifacts/mosslight-preview/src/hooks/use-preferences.ts`
- `artifacts/mosslight-preview/src/pages/home.tsx`
- `artifacts/mosslight-preview/src/pages/plants.tsx`
- `artifacts/mosslight-preview/src/pages/plant-detail.tsx`
- `artifacts/mosslight-preview/src/pages/learn.tsx`
- `artifacts/mosslight-preview/src/pages/journal.tsx`
- `artifacts/mosslight-preview/src/pages/profile.tsx`
- `artifacts/mosslight-preview/src/index.css`
- `artifacts/mosslight-preview/vite.config.ts`
- `artifacts/mobile/app/(tabs)/index.tsx`
- `artifacts/mobile/app/(tabs)/garden.tsx`
- `artifacts/mobile/app/(tabs)/discover.tsx`
- `artifacts/mobile/app/(tabs)/journal.tsx`
- `artifacts/mobile/app/(tabs)/profile.tsx`
- `artifacts/mobile/app/add-plant.tsx`
- `artifacts/mobile/app/import-plant.tsx`
- `artifacts/mobile/app/plant/[id].tsx`
- `artifacts/mobile/app/encyclopedia/[speciesId].tsx`
- `artifacts/mobile/app/identify/[speciesId].tsx`
- `artifacts/mobile/app/settings.tsx`
- `artifacts/mobile/context/GardenContext.tsx`
- `artifacts/mobile/components/PlantSeatGuide.tsx`
- `artifacts/mobile/components/PlantPreviewSheet.tsx`
- `artifacts/mobile/data/tips.ts`
- `artifacts/mobile/data/field-journal.ts`
- `artifacts/mobile/data/soil-guide.ts`
- `artifacts/mobile/data/pests.ts`
- `artifacts/mobile/data/plants.ts`
- `artifacts/api-server/src/routes/plants.ts`

---

#### #234 — Verify the native Learn journey before TestFlight resubmission
- **Status:** MERGED
- **Created:** 2026-08-25T07:19:36.588Z
- **Updated:** 2026-08-25T07:26:43.529Z
- **Category:** test_gaps

# Verify the native Learn journey before TestFlight resubmission

## What & Why
The iOS Learn catalogue and ordering now pass static checks, but a real-device review is still needed to confirm the user-facing experience before release. This is especially important for the new Sources & methodology modal and the long, accessibility-aware content cards.

## Done looks like
- Review the Learn tab on a physical iOS device or iOS simulator from the first category through Pet & Child Safety
- Confirm the 25-category order, accordion expansion, read-progress state, source modal scrolling, external links, and back navigation
- Check larger text/accessibility labels and confirm no clipping, inaccessible controls, or accidental duplicate content
- Record and resolve any native-only issues before the next TestFlight submission

## Relevant files
- artifacts/mobile/app/(tabs)/discover.tsx
- artifacts/mobile/components/SourcesMethodologyCard.tsx
- artifacts/mobile/data/tips.ts
- artifacts/mobile/data/sources.ts

---

#### #236 — Prevent a stale iOS build from being sent to TestFlight
- **Status:** MERGED
- **Created:** 2026-08-25T07:44:37.429Z
- **Updated:** 2026-08-25T08:10:04.182Z
- **Category:** tech_debt

# Prevent a stale iOS build from being sent to TestFlight

## What & Why
Make the release workflow run the iOS build to completion before starting TestFlight submission, so `--latest` cannot select an older or already-uploaded binary.

## Done looks like
- The combined project workflow no longer starts iOS build and TestFlight submission in parallel
- A submission can only start after a successful iOS build
- The workflow output clearly reports the build number being submitted
- A retry does not produce Apple error 90189 for a stale build

## Out of scope
- Changes to Learn content
- Changes to the browser preview
- Public App Store release

## Relevant files
- `.replit`
- `scripts/bump-build-number.mjs`
- `artifacts/mobile/app.config.ts`

---

#### #241 — Reduce the mobile plant-art upload safely
- **Status:** MERGED
- **Created:** 2026-08-27T23:52:08.397Z
- **Updated:** 2026-08-28T00:12:17.151Z

# Reduce plant asset footprint

## What & Why
Reduce the iOS/Android release archive without changing the plants users can browse or the offline fallback behavior they rely on. The current mobile archive is about 1.6 GB, driven almost entirely by plant artwork, so the cleanup must distinguish genuinely unused files from assets needed by static imports or offline paths.

## Done looks like
- A complete report identifies every plant image referenced by the native photo map and every file physically present in the plant asset directory.
- Orphaned files and artwork for species outside the live 185-species catalog are removed only after confirming there are no dynamic path-based references.
- Remaining fallback artwork is resized for its real display surfaces and converted to WebP where the supported native image stack loads it reliably.
- Native image loading remains remote-first, with a bundled fallback for live catalog plants when the network or API is unavailable.
- Mobile typecheck, catalog validation, Metro bundling, and a local EAS archive-size dry run pass.
- No EAS build, upload, or TestFlight submission is started as part of validation.

## Out of scope
- Removing live plant species from the catalog or changing the 185-species published set.
- Removing production critter, onboarding, lighting, or UI artwork.
- Deleting bundled artwork before static and dynamic reference checks pass.
- Starting a cloud EAS build or uploading a release archive.

## Steps
1. **Inventory references safely** -- Parse every static photo import and inspect the image resolver for dynamic path construction or aliases. Produce orphan and duplicate reports before deleting anything.
2. **Align fallbacks with the live catalog** -- Cross-reference imported species IDs with the published 185-species set and retain only fallback artwork that can be reached by the current app, while preserving every live ID and alias that the UI can request.
3. **Optimize the retained artwork** -- Resize images to the largest actual native display size and convert to WebP where iOS, Android, Expo Image, and Metro all support the result; otherwise use lossless/lower-cost PNG optimization without changing lookup behavior.
4. **Verify remote-first fallback behavior** -- Confirm the remote catalog URL is attempted first, a failed remote image falls back locally, and offline/live-catalog screens still render without missing-module or missing-asset errors.
5. **Measure without uploading** -- Run native validation and Metro production bundling, then create a local archive-size report honoring the mobile EAS ignore rules. Stop there unless the user separately requests a cloud build.

## Relevant files
- `artifacts/mobile/data/plant-photos.ts`
- `artifacts/mobile/data/plants.ts`
- `artifacts/mobile/lib/plantCatalog.ts`
- `artifacts/mobile/context/PlantCatalogContext.tsx`
- `artifacts/mobile/components/PlantImage.tsx`
- `artifacts/mobile/assets/plants`
- `artifacts/mobile/.easignore`
- `artifacts/api-server/src/data/published-plant-ids.ts`
- `artifacts/api-server/src/data/plants.json`
- `artifacts/api-server/src/routes/plants.ts`
- `artifacts/mobile/scripts/build.js`

---

#### #243 — Replace onboarding with a lightweight guided tour
- **Status:** MERGED
- **Created:** 2026-08-28T03:34:40.863Z
- **Updated:** 2026-08-28T04:35:14.141Z

# Lightweight Guided Tour\n\n## What & Why\nReplace the current multi-layer onboarding experience with one calm, lightweight guided tour. Guests and signed-in members should understand the core MossLight experience without being interrupted by a long carousel, chained tooltips, or repeated first-visit cards; people who skip should be able to opt back in later from Profile.\n\n## Done looks like\n- First-time guests and signed-in members see one concise welcome/tour entry point rather than the current intro carousel followed by another feature tour.\n- The guided tour has a small number of useful steps covering the greenhouse, tending, and discovery/journal experience, with short copy, clear progress, and a quiet visual treatment.\n- “Skip for now” exits immediately without a confirmation dialog or leaving later tooltip interruptions behind.\n- Completing or skipping the tour prevents automatic reappearance on that device, while Profile includes a clear “Watch the tour again” action for guests and members.\n- Replaying from Profile opens the same tour and resets its transient progress without changing the user’s garden, account, or onboarding name data.\n- The old chained onboarding tooltips and screen-specific first-entry tour cards no longer appear during normal navigation.\n- The flow remains usable with larger text, small phone heights, keyboard-safe layouts, swipe/tap navigation where retained, and reduced interaction friction on web/Expo.\n\n## Out of scope\n- Changes to authentication, guest session types, account sync, or garden data.\n- New product education beyond the core greenhouse, tending, discovery, and journal concepts.\n- Changes to the existing plant-detail help content, care copy, or profile sections unrelated to onboarding.\n- Analytics, remote-config experiments, or a server-synchronized tour history.\n\n## Steps\n1. **Consolidate the entry experience** -- Rework the first-run welcome into a brief invitation to explore, keeping name capture optional and making “take the tour” and “skip for now” clear, low-pressure choices.\n2. **Build the single replayable tour** -- Replace the long feature sequence with a short guided tour that communicates the highest-value actions, uses consistent controls, and can be launched both after first entry and from Profile.\n3. **Retire competing overlays** -- Remove the automatic screen-specific TourCard and OnboardingTooltip presentations, including their chained prerequisite behavior, so users are not interrupted after the main tour.\n4. **Connect persistence and replay** -- Keep onboarding completion/skip state device-local, migrate or neutralize legacy keys so old users do not get surprise overlays, and connect the Profile action to the existing replay request path for both guest and signed-in sessions.\n5. **Verify the complete matrix** -- Test fresh guest entry, signed-in entry, skip, finish, replay, guest-to-account transition, relaunch persistence, small screens, and web/Expo typechecks plus rendered screens.\n\n## Relevant files\n- `artifacts/mobile/components/WelcomeScreen.tsx`\n- `artifacts/mobile/components/FeatureTour.tsx`\n- `artifacts/mobile/components/LookAroundTour.tsx`\n- `artifacts/mobile/components/OnboardingTooltip.tsx`\n- `artifacts/mobile/components/TourCard.tsx`\n- `artifacts/mobile/app/_layout.tsx`\n- `artifacts/mobile/app/(tabs)/profile.tsx`\n- `artifacts/mobile/app/settings.tsx`\n- `artifacts/mobile/context/GardenContext.tsx`\n- `artifacts/mobile/app/(tabs)/index.tsx`\n- `artifacts/mobile/app/(tabs)/discover.tsx`\n- `artifacts/mobile/app/(tabs)/garden.tsx`\n- `artifacts/mobile/app/(tabs)/journal.tsx`\n- `artifacts/mobile/app/plant/[id].tsx`

---

#### #245 — Clean Up Your Conservatory
- **Status:** MERGED
- **Created:** 2026-08-28T04:16:00.114Z
- **Updated:** 2026-08-28T04:32:08.875Z
- **Artifact kinds:** mobile

# Clean Up Conservatory

## What & Why
Add a safe, garden-voiced bulk cleanup flow to the mobile Conservatory so users can select several plants from either list or grid presentation and remove them together without accidental deletion.

## Done looks like
- Users can enter **Clean Up Your Conservatory** selection mode from a dedicated header action and the existing long-press interaction pattern.
- Selection mode clearly shows the selected count, lets users select/deselect individual plants, and works in both list and grid views.
- A dedicated confirmation state uses the existing single-plant deletion confirmation pattern before any records are removed.
- Garden Points are not deducted or recalculated when plants are removed.
- Journal entries remain as historical records; entries tied to removed plants are retained and visibly labeled as belonging to a removed plant rather than silently disappearing.
- Archived plants created by Estate Preview slot-capacity reversion can be selected and removed through the same flow.
- Empty selection, cancel, back navigation, and removing the final plant leave the Conservatory in a coherent state.
- Accessibility labels, roles, and test IDs identify selection mode, selected count, plant toggles, cancel, and confirmation actions.

## Out of scope
- Changing single-plant deletion semantics beyond sharing its confirmation styling and wording pattern.
- Deducting historical Garden Points or deleting Journal history.
- Changing the Estate Preview archive/revert rules or plant capacity calculations.
- Bulk editing plant details, moving plants, or adding a new server-side deletion API.

## Steps
1. **Map the existing interaction model** -- Reuse the Conservatory’s existing list/grid rendering and long-press behavior, while preserving normal tap-to-open behavior outside selection mode.
2. **Build selection mode** -- Add the garden-voiced entry action, selected-count header, individual selection toggles, select-all/clear affordances where useful, and consistent behavior for active and archived plants.
3. **Add safe confirmation and deletion** -- Present a dedicated bulk confirmation state before removing anything, remove all selected plant records atomically from the local garden, preserve Garden Points, and retain Journal entries with a removed-plant historical label.
4. **Cover responsive and accessible states** -- Validate list/grid layouts, empty selections, final-item removal, safe areas, large text, keyboard/search interaction, and screen-reader labels.
5. **Verify regressions** -- Run mobile typecheck and existing catalog validations, exercise active and archived records, and confirm single-plant deletion and journal history remain intact.

## Relevant files
- `artifacts/mobile/app/(tabs)/garden.tsx`
- `artifacts/mobile/components/PlantCard.tsx`
- `artifacts/mobile/context/GardenContext.tsx`
- `artifacts/mobile/hooks/useJournal.ts`
- `artifacts/mobile/app/(tabs)/journal.tsx`
- `artifacts/mobile/app/plant/[id].tsx`
- `artifacts/mobile/data/plants.ts`

---

#### #247 — Restore critical Profile, care, and tour flows
- **Status:** MERGED
- **Created:** 2026-08-28T04:49:29.860Z
- **Updated:** 2026-08-28T12:33:11.867Z

# Restore Critical App Flows

## What & Why
Restore and verify four release-critical user flows: make signed-in account deletion unmistakably visible, initialize new plants from accurate care dates, align Settings and replay behavior with the intended seven-stage guided walkthrough, and fix clipping triggered by MossLight's own Large and Extra Large text settings. Account deletion is still implemented but currently sits above Privacy Policy and is conditional on a signed-in Clerk user; the recent onboarding merge did not remove it. The merged tour, however, is currently a three-step full-screen feature carousel rather than the documented stateful walkthrough over real app routes, so changing its Settings description alone would be inaccurate.

## Done looks like
- Signed-in users see a clearly destructive **Delete account** action directly below Privacy Policy at the visible end of Profile, with accessible labeling, test coverage, and the existing two-stage irreversible confirmation.
- Guest users do not see an account-deletion action because they have no remote account; their sign-out/return flow remains unchanged.
- Account deletion remains functional and clears the authenticated account and user-scoped local state without being hidden by unrelated Profile content.
- The add-plant configuration asks **When did you last water this?** with an optional date choice; skipping uses today, and the new plant's next watering date is calculated from that baseline instead of appearing immediately due.
- The same flow offers **When did you last fertilize this?**. A supplied date drives the next fertilizing date; skipping schedules forward from the add date without fabricating a fertilizing event, incrementing care totals, or awarding Garden Points.
- Quantity adds and other plant creation paths preserve supplied care dates and use the same safe defaults when dates are absent.
- Settings describes the current seven-stage walkthrough rather than an old three- or five-step tour.
- Tapping **Watch the tour again** launches the stateful walkthrough in replay/sandbox mode, follows the connected add-plant → Find the Right Seat → Conservatory search → Journal → Learn → Greenhouse sequence, and never creates a duplicate real plant.
- The first-run name capture remains intact and continues to populate the Greenhouse greeting.
- The app is exercised with its in-app Text Size setting at both Large and Extra Large across primary navigation, Profile/Settings, add-plant, guided walkthrough, and confirmation/modal surfaces; icon/text clipping or inaccessible controls are fixed through shared responsive layout behavior rather than one-off hiding or truncation.

## Out of scope
- Internationalization work, package installation, or translation extraction; i18n remains deferred until explicitly approved.
- Translation or rewriting of Herbarium/Learn scientific content.
- Redesigning the account-deletion policy or adding deletion for guest-only sessions.
- Awarding historical Garden Points for care dates entered during plant creation.
- A broad visual redesign unrelated to text-size clipping and touch accessibility.

## Steps
1. **Restore account-deletion visibility** -- Move the signed-in deletion action to the true end of the Profile privacy/account area, preserve its confirmation behavior, and verify that recent onboarding changes did not alter its handler.
2. **Capture initial care dates safely** -- Add optional watering and fertilizing date inputs to plant setup, pass the values through all relevant creation paths, and calculate future care from an honest baseline without creating false care activity.
3. **Correct tour replay architecture and copy** -- Replace the misleading three-step replay path with the documented connected walkthrough and sandbox behavior, retain name capture, and update Settings/Profile descriptions to match what actually launches.
4. **Audit in-app large text modes** -- Exercise Large and Extra Large independently of OS Dynamic Type, repair shared layout constraints causing clipping, and verify the affected primary screens and walkthrough states.
5. **Validate release-critical behavior** -- Run mobile typechecking and focused regression checks for deletion visibility, care scheduling, replay isolation, name persistence, navigation continuity, and both in-app text-size choices.

## Relevant files
- `artifacts/mobile/app/(tabs)/profile.tsx:470-560,1630-1881`
- `artifacts/mobile/app/settings.tsx:34-64,159-396`
- `artifacts/mobile/app/add-plant.tsx:115-270,291-540`
- `artifacts/mobile/context/GardenContext.tsx:25-44,1173-1255,1506-1527`
- `artifacts/mobile/context/AccessibilityContext.tsx`
- `artifacts/mobile/lib/applyFontScale.ts`
- `artifacts/mobile/app/_layout.tsx:365-445`
- `artifacts/mobile/components/FeatureTour.tsx`
- `artifacts/mobile/components/WelcomeScreen.tsx`
- `artifacts/mobile/components/PlantSeatGuide.tsx`
- `artifacts/mobile/app/(tabs)/garden.tsx`
- `artifacts/mobile/app/(tabs)/journal.tsx`
- `artifacts/mobile/app/(tabs)/discover.tsx`

---

#### #249 — Fix live onboarding and sign-in regressions
- **Status:** MERGED
- **Created:** 2026-08-28T16:54:06.390Z
- **Updated:** 2026-08-28T17:42:05.849Z
- **Artifact kinds:** mobile

# Fix Live Onboarding Regressions

## What & Why
Fix four regressions confirmed on TestFlight 2.1.0 build 5: the Find the Right Seat catalog is empty, the seven-stage guided tour cannot be completed reliably, the social buttons use the wrong action wording, and the sign-in screen scrolls when its content fits. The prior tour checks only inspected source strings and did not execute the user-visible flow, so this work must prove rendered behavior rather than relying on typechecks alone.

## Done looks like
- Find the Right Seat returns matching plants for a known query on a production-configured build, with a bundled fallback and an explicit recoverable error when the remote catalog is unavailable
- The tour starts from its visible primary action, advances through all seven stages without a dead end, survives the transparency gate and replay timing, and replay creates no persistent plant or care changes
- Social buttons read “Sign in with Apple” and “Sign in with Google”; the Apple button uses Apple-approved wording and preserves compliant branding
- The sign-in screen cannot be dragged when its rendered content fits, but scrolling becomes available when the keyboard, small viewport, or accessibility text size causes real overflow
- Executable route/state tests cover the complete tour and catalog failure path instead of checking only source strings
- Separate rendered evidence is captured for each fix before release
- A paid EAS build and TestFlight submission happen only after the user gives a new, explicit approval for those exact actions
- Final physical-device confirmation is based on the user installing the approved TestFlight build and supplying a screenshot or recording for each case; no simulator or browser capture is represented as device proof

## Out of scope
- i18n
- A broader sign-in redesign
- Replacing Clerk or changing login providers
- Unrelated Herbarium, greenhouse, or accessibility enhancements
- Claiming access to, control of, or recording from the user’s physical iPhone
- Any EAS build or App Store/TestFlight submission without a separate explicit approval immediately before the paid action

## Steps
1. **Repair catalog loading** — Expose the resolved production catalog request outcome, correct any endpoint/configuration failure, and make the seat guide search the bundled catalog whenever remote data is unavailable or empty.
2. **Make the seven stages executable** — Ensure every stage is reachable from the visible primary action, remove the stage-two bypass and stale-route behavior, and make first-run and replay transitions deterministic across modal gates and remounts.
3. **Protect replay data** — Exercise the complete replay flow and verify that it does not create plants, locations, care totals, points, or completion side effects intended only for first run.
4. **Replace static tour checks** — Add executable tests that drive route parameters, persistence state, catalog success/failure, and all seven user-visible transitions.
5. **Correct provider labels** — Change the Apple and Google actions to “Sign in with Apple” and “Sign in with Google,” using Apple’s official sign-in wording and retaining compliant logo, contrast, sizing, and clear-space treatment.
6. **Fix overflow behavior** — Remove the flex-based content measurement feedback, recalculate overflow after layout and keyboard changes, and enable scrolling only when the rendered content genuinely exceeds the available viewport.
7. **Capture pre-release evidence** — Run the mobile app with production-equivalent configuration and capture separate rendered screenshots or recordings showing catalog results, the complete tour, both provider labels, and the non-scrolling/real-overflow sign-in states.
8. **Release only with approval** — Present the local evidence and request explicit authorization for one paid iOS build and one TestFlight submission. After the user installs that build, use the user-provided device captures to verify each issue individually before declaring the task resolved.

## Relevant files
- `artifacts/mobile/components/PlantSeatGuide.tsx`
- `artifacts/mobile/context/PlantCatalogContext.tsx`
- `artifacts/mobile/lib/plantCatalog.ts`
- `artifacts/mobile/app.config.ts`
- `artifacts/mobile/app/_layout.tsx`
- `artifacts/mobile/components/WelcomeScreen.tsx`
- `artifacts/mobile/app/add-plant.tsx`
- `artifacts/mobile/app/(tabs)/discover.tsx`
- `artifacts/mobile/app/(tabs)/garden.tsx`
- `artifacts/mobile/app/(tabs)/journal.tsx`
- `artifacts/mobile/app/(tabs)/index.tsx`
- `artifacts/mobile/app/settings.tsx`
- `artifacts/mobile/context/GardenContext.tsx`
- `artifacts/mobile/scripts/validate-critical-flows.ts`
- `artifacts/mobile/app/(auth)/sign-in.tsx`

---

#### #251 — Refine first-run and replay walkthrough
- **Status:** MERGED
- **Created:** 2026-08-28T17:43:40.364Z
- **Updated:** 2026-08-28T18:12:03.624Z
- **Artifact kinds:** mobile

# Refine Guided Walkthrough

## What & Why
Correct MossLight’s walkthrough so first-time onboarding creates a real garden while later replays remain completely read-only. Improve the teaching flow with field-level pointers during plant setup, route stage 6 to the Herbarium Species panel instead of repeating the light guide, and prove that replay cannot alter watering, Journal, or Garden Points data.

## Done looks like
- A plant added during first-time onboarding is saved to the active guest-local or signed-in garden, appears in Conservatory and Greenhouse afterward, and can be managed like any other plant
- Replaying from Profile remains session-wide read-only across plants, care history and totals, Journal, Garden Points, Learn progress, species progress, photos, locations, unlocks, and every other persistent action
- Replay mode visibly or behaviorally blocks watering an existing plant, adding a Journal entry, and completing the Garden Points daily check-in
- Before/after persistence evidence confirms those three replay interactions write nothing to the real garden or account
- Stage 6 opens the Herbarium Species panel and teaches a new surface rather than returning to Light & Seasons
- Stage 1 configuration shows one small dismissible botanical pointer at a time for Nickname, How many?, Location, and Last watered, while preserving the existing whole-stage transition cards
- The pointers remain usable at supported text sizes and on narrow phone screens
- Rendered evidence covers first-run persistence, the four pointers, the corrected stage-6 destination, the three blocked replay actions, and normal writes resuming after replay ends
- No paid EAS build or TestFlight submission occurs without separate explicit approval

## Out of scope
- Weakening replay safety to allow any real write
- Replacing the existing seven-stage route sequence or transition-card pattern
- i18n
- A broader redesign of Add Plant, Herbarium, Journal, or Greenhouse
- Paid build or submission actions

## Steps
1. **Separate first-run from replay behavior** — Audit walkthrough state and persistence gates so first-run uses normal garden mutations while replay activates the existing session-wide read-only boundary.
2. **Add field-level guidance** — Build a reusable botanical pointer and sequence it beside the four requested plant-configuration fields with dismiss and advance behavior.
3. **Correct the final learning destination** — Change stage 6 to activate and explain the Herbarium Species panel without repeating stage 3.
4. **Strengthen replay interaction blocking** — Ensure watering, Journal creation, daily check-in, and any lower-level persistence paths remain blocked for the full replay session.
5. **Prove both modes** — Run first-run and replay browser flows with seeded state, compare persisted values before and after, revisit normal screens after replay, and capture rendered evidence for each requested behavior.
6. **Validate and review** — Run mobile typechecking and critical-flow checks, inspect runtime logs, and request a fresh architectural review before completion.

## Relevant files
- `artifacts/mobile/lib/guidedTour.ts`
- `artifacts/mobile/app/_layout.tsx`
- `artifacts/mobile/app/add-plant.tsx`
- `artifacts/mobile/app/(tabs)/discover.tsx`
- `artifacts/mobile/app/(tabs)/garden.tsx`
- `artifacts/mobile/app/(tabs)/journal.tsx`
- `artifacts/mobile/app/(tabs)/index.tsx`
- `artifacts/mobile/context/GardenContext.tsx`
- `artifacts/mobile/hooks/useJournal.ts`
- `artifacts/mobile/hooks/useDailyPrompt.ts`
- `artifacts/mobile/components/PlantCard.tsx`
- `artifacts/mobile/scripts/validate-critical-flows.ts`
- `scripts/capture-tour-evidence.mjs`

---

#### #253 — Make Apple sign-in work before showing it again
- **Status:** MERGED
- **Created:** 2026-08-28T23:51:34.672Z
- **Updated:** 2026-08-28T23:54:17.253Z
- **Category:** next_steps

# Make Apple sign-in work before showing it again

## What & Why
Apple sign-in is currently disabled because the Clerk instance does not accept the oauth_apple strategy. Configure Apple as a Social Connection in the Production Replit-managed Clerk instance, then verify the complete login and account-linking flow on a physical iPhone before re-enabling the UI. Clerk dashboard access currently requires an active personal Replit Pro subscription.

## Done looks like
- Apple is enabled and fully configured in the Production Clerk instance
- A real Apple sign-in succeeds on a physical iPhone, returns to MossLight, activates the Clerk session, and completes the existing garden merge/restore path
- Cancellation and provider errors return the user safely to sign-in
- APPLE_SSO_ENABLED is changed to true only after the end-to-end test passes
- A new iOS build is created/distributed only after explicit approval

## Relevant file
- artifacts/mobile/app/(auth)/sign-in.tsx

---

#### #255 — Security scan
- **Status:** MERGED
- **Created:** 2026-08-29T00:33:07.141Z
- **Updated:** 2026-08-29T00:49:43.592Z

Run an in-depth scan across the entire project

---

#### #256 — Purchase and entitlement integrity
- **Status:** MERGED
- **Created:** 2026-08-29T00:46:26.536Z
- **Updated:** 2026-08-29T01:05:38.807Z

Paid tiers and companion skins must be derived from trusted purchase state and correctly isolated across accounts and devices.

Vulnerabilities to fix:

1. [Medium] Profile sync accepts forged premium-skin entitlements
  Any authenticated free user can add paid companion skins to their own account without making an App Store purchase.

The profile-sync API accepts every JSON object supplied by the authenticated caller as `snapshot` without an allowlist, schema validation, or separation of server-authoritative entitlements (`artifacts/api-server/src/routes/profile.ts:195-224`). An attacker can obtain their own valid greenhouse code through `POST /api/profile/code`, then issue an authenticated `PUT /api/profile/<their-code>` containing a forged `@quietgrove:purchasedPremiumSkins` value such as `[\"sakura-mantis\"]` and matching active-skin data. The client includes this key in cloud snapshots (`artifacts/mobile/utils/api.ts:13-44`), restores it from server data without purchase verification (`artifacts/mobile/context/GardenContext.tsx:865-872` and `1895-1936`), and uses it as the premium UI gate (`artifacts/mobile/app/(tabs)/profile.tsx:1201-1203`). `equipSkin` does not independently require a RevenueCat entitlement (`GardenContext.tsx:1190-1204`), and the RevenueCat reconciliation only adds entitlements; it does not remove forged entries (`artifacts/mobile/app/_layout.tsx:296-308`).

Authentication confines the write to the attacker's own profile, so this is not an IDOR. It is nevertheless a direct bypass of the paid digital-goods control: a normal account plus a documented authenticated API request can persist and use premium skins, and the forged value survives later RevenueCat refreshes and profile sync.
  Files: artifacts/api-server/src/routes/profile.ts, artifacts/mobile/utils/api.ts, artifacts/mobile/context/GardenContext.tsx, artifacts/mobile/app/_layout.tsx, artifacts/mobile/app/(tabs)/profile.tsx

2. [Medium] RevenueCat purchases remain attached to a device across account switches
  On a shared device, a person who signs in after a paying customer can inherit the customer's paid skins and subscription tier instead of being evaluated against their own MossLight account.

RevenueCat is configured once with only an API key (`artifacts/mobile/lib/revenuecat.tsx:141-149`). The app never calls `Purchases.logIn` with the Clerk user ID after authentication or `Purchases.logOut` when the Clerk session ends. RevenueCat therefore retains its installation-level anonymous customer identity across distinct MossLight accounts. `performSignOut` clears AsyncStorage but does not reset RevenueCat (`artifacts/mobile/app/(tabs)/profile.tsx:491-508`).

After account A purchases an entitlement and signs out, `RevenueCatSync` reads the still-active anonymous `CustomerInfo` and copies its skins into local state (`artifacts/mobile/app/_layout.tsx:296-308`) and applies its subscription tier (`app/_layout.tsx:331-345`). The copied skin list is accepted by the premium-access UI (`profile.tsx:1201-1203`) and is included in the next profile snapshot (`GardenContext.tsx:964-998`), so the entitlement can persist into account B's cloud profile rather than only being displayed transiently.

Exploitation requires access to a device after a paying user signs out; it does not expose arbitrary remote accounts. It does defeat account-level purchase attribution and lets a non-paying account retain paid digital goods obtained from a prior device user.
  Files: artifacts/mobile/lib/revenuecat.tsx, artifacts/mobile/app/_layout.tsx, artifacts/mobile/app/(tabs)/profile.tsx, artifacts/mobile/context/GardenContext.tsx

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #263 — Repair TestFlight release blockers
- **Status:** MERGED
- **Created:** 2026-08-29T05:37:46.104Z
- **Updated:** 2026-08-29T12:38:20.881Z
- **Artifact kinds:** mobile

# Repair TestFlight Release Blockers

## What & Why
Repair the regressions demonstrated in the current TestFlight walkthrough so MossLight can move through review with a trustworthy first-use experience. The work must restore functioning account deletion, correct first-plant care dates, reconnect the seven-stage guided journey, and make Find the Right Seat understandable when the remote catalog is unavailable. The release must continue using Clerk and RevenueCat, must not introduce Apple sign-in as a requirement, and must not publish the Replit API deployment.

## Done looks like
- A signed-in Google or email user can complete the two-stage Delete account flow; PostgreSQL profile data and the Clerk account are deleted before local state and app-owned photos are cleared.
- A failed deletion preserves local data, reports the actionable server reason without exposing sensitive details, and can be retried.
- Account deletion and other server-backed release checks use a reachable configured HTTPS API; no Replit Publish action is performed.
- A plant added without a previous watering date starts its watering schedule from the add date and does not appear immediately due or overdue.
- Explicit watering and fertilizing dates continue to produce the correct next-care dates without fabricating care events, totals, points, or rewards.
- A true first-run account enters the connected seven-stage walkthrough; a returning account is not incorrectly forced through it.
- Replay launches the same seven stages in a strict read-only session and cannot change garden, care, Journal, Learn, Species, points, rewards, location, photo, notification, or sync data.
- The plant added in stage one remains selected and clearly named when Find the Right Seat opens.
- Find the Right Seat never says search is unavailable while local search works; remote failure is presented as a non-blocking on-device-catalog notice that clears or updates correctly.
- Plant imagery no longer hides the plant name, care needs, placement explanation, or continuation controls, and users can clearly tell that more content is available below.
- The Conservatory search, Journal privacy, Learn/Species, and Greenhouse-return stages advance in order and complete cleanly.
- Export data finishes with a verified ZIP and opens the native share sheet, or shows an actionable failure without remaining stuck in a loading state.
- Premium tiers and skins remain derived only from the active Clerk user's RevenueCat state; synced companion progress cannot grant paid-only companions.
- Native validation covers the demonstrated regression paths, the app starts cleanly, the iOS build number is advanced once, and a successful production EAS build is submitted to TestFlight sequentially.

## Out of scope
- Enabling or requiring Apple sign-in.
- Publishing or republishing the Replit API deployment.
- Browser-only parity work unrelated to the native TestFlight release.
- Redesigning the entire Profile, tier catalog, or companion store.
- Changing subscription prices, RevenueCat products, Clerk providers, or the seven-stage product sequence.

## Steps
1. **Diagnose the release backend boundary** -- Trace the TestFlight API base and reproduce account deletion and health failures against the configured service. Repair the client/server contract where possible without Replit publishing, and treat an unreachable backend as a hard release blocker rather than silently weakening deletion.
2. **Restore safe account deletion and export** -- Verify the authenticated database-then-Clerk deletion sequence, preserve retry-safe local behavior, improve actionable failure handling, and confirm native ZIP export reaches the share sheet and always clears its loading state.
3. **Correct initial care scheduling** -- Reproduce the immediate-overdue state and make every plant creation path use the add date when watering is skipped while preserving explicit historical dates and fertilizing semantics.
4. **Reconnect first-run and replay routing** -- Validate clean-account eligibility, returning-account behavior, all seven route transitions, selected-plant context, completion, cancellation, and strict replay isolation.
5. **Repair Find the Right Seat usability** -- Separate remote-catalog status from local-search availability, carry the selected plant into the guide, reduce image dominance, surface essential care and placement text above the fold, and make scrolling and the next action obvious.
6. **Harden paid companion boundaries** -- Filter synced companion progress so ordinary profile data cannot grant paid-only companion types while preserving verified RevenueCat ownership, owner-only testing, and free progression.
7. **Add regression validation** -- Extend native critical-flow validation for deletion failure/success boundaries, first-plant due dates, seven-stage routing, replay non-persistence, fallback search messaging, selected-plant continuity, and premium companion filtering.
8. **Build and submit sequentially** -- Restart and inspect the mobile and API workflows, run the mobile validation suite, visually verify the affected native screens where available, advance the iOS build number once, run the production EAS build, and submit that exact completed build to TestFlight only after all release gates pass.

## Relevant files
- `artifacts/mobile/app/(tabs)/profile.tsx`
- `artifacts/mobile/utils/api.ts`
- `artifacts/api-server/src/routes/profile.ts`
- `artifacts/mobile/utils/exportGarden.ts`
- `artifacts/mobile/app/add-plant.tsx`
- `artifacts/mobile/context/GardenContext.tsx`
- `artifacts/mobile/lib/plantCareSchedule.ts`
- `artifacts/mobile/components/PlantSeatGuide.tsx`
- `artifacts/mobile/context/PlantCatalogContext.tsx`
- `artifacts/mobile/app/(tabs)/discover.tsx`
- `artifacts/mobile/app/(tabs)/garden.tsx`
- `artifacts/mobile/app/(tabs)/journal.tsx`
- `artifacts/mobile/app/_layout.tsx`
- `artifacts/mobile/lib/guidedTour.ts`
- `artifacts/mobile/lib/revenuecat.tsx`
- `artifacts/mobile/scripts/validate-critical-flows.ts`
- `artifacts/mobile/app.config.ts`
- `artifacts/mobile/eas.json`

---

#### #264 — Verify the TestFlight build on a physical iPhone
- **Status:** MERGED
- **Created:** 2026-08-29T12:33:31.123Z
- **Updated:** 2026-08-29T12:42:25.915Z
- **Depends on:** #263
- **Proposed from:** #263
- **Category:** test_gaps

# Verify the TestFlight build on a physical iPhone

## What & Why
Build 14 was accepted by App Store Connect and is undergoing Apple processing. A physical-device pass is still needed for the complete release confidence check because the workspace cannot exercise native iOS hardware behavior.

## Done looks like
- Build 14 finishes Apple processing and is installable from TestFlight.
- The seven-stage first-run walkthrough creates exactly one plant and reaches the Greenhouse.
- Replay remains read-only for plants, care history, Journal, photos, points, rewards, location, sync, and notifications.
- Native export success/cancel/failure and account deletion ordering behave correctly.

## Relevant files
- artifacts/mobile/app/_layout.tsx
- artifacts/mobile/app/add-plant.tsx
- artifacts/mobile/app/(tabs)/discover.tsx
- artifacts/mobile/app/(tabs)/garden.tsx
- artifacts/mobile/app/(tabs)/journal.tsx
- artifacts/mobile/app/(tabs)/profile.tsx

---

#### #266 — Restore Conservatory add and filter tools
- **Status:** MERGED
- **Created:** 2026-08-29T13:27:56.886Z
- **Updated:** 2026-08-29T13:41:35.444Z
- **Artifact kinds:** mobile

# Restore Conservatory Add Tools

## What & Why
Restore the missing discovery, filtering, request, location, and layout tools in Add to Conservatory so people can quickly find one of the 185 published plants or clearly request a missing species.

## Done looks like
- Searching for a plant outside the active 185 shows a prominent “Want a specific plant?” contact/request action directly in Add to Conservatory.
- “Import via code” remains visible but is clearly marked Coming soon and cannot open the unfinished import/search flow.
- Catalog controls include water sensitivity (filtered, tap, or distilled), useful difficulty choices, plant type/category, and family without forcing people to scroll through one enormous list.
- Sorting labels match their actual direction and behavior.
- The Add to Conservatory layout stacks cleanly on small iPhones and keeps the primary “Begin tending” action visible.
- New accounts start with Kitchen, Living Room, and Office; people can add, rename, and delete their own locations, and custom locations persist locally and sync with their account.
- Morning and Afternoon fill the seat-guide control evenly across the available width.

## Out of scope
- Implementing plant-share code import.
- Adding species beyond the active 185.
- Changing paid plant-slot limits.

## Steps
1. **Rebuild catalog controls** -- Add compact, accessible filtering and sorting for water sensitivity, difficulty, category/type, and family while preserving search across common and scientific names.
2. **Add the missing-plant request path** -- Place the request action in the empty-search state and make the contact route clear and easy to use.
3. **Pause unfinished code import** -- Replace the active import flow with a clear Coming soon state without removing the future entry point.
4. **Simplify location setup** -- Use the three requested defaults and provide durable custom-location management across add, edit, and Conservatory views.
5. **Fix constrained layouts** -- Stack Conservatory controls and walkthrough overlays safely, keep primary actions tappable, and make the seat time buttons equal width.
6. **Add regression coverage** -- Verify the 185-species boundary, each filter, missing-result request state, location lifecycle, and narrow-screen layouts.

## Relevant files
- `artifacts/mobile/app/add-plant.tsx`
- `artifacts/mobile/app/import-plant.tsx`
- `artifacts/mobile/app/(tabs)/discover.tsx`
- `artifacts/mobile/app/(tabs)/index.tsx`
- `artifacts/mobile/app/plant/[id].tsx`
- `artifacts/mobile/components/PlantSeatGuide.tsx`
- `artifacts/mobile/context/GardenContext.tsx`
- `artifacts/mobile/data/plants.ts`
- `artifacts/mobile/lib/plantCatalog.ts`

---

#### #267 — Repair the complete seven-stage walkthrough
- **Status:** MERGED
- **Created:** 2026-08-29T13:27:56.886Z
- **Updated:** 2026-08-29T14:00:16.636Z
- **Depends on:** #266
- **Artifact kinds:** mobile

# Repair Complete Walkthrough

## What & Why
Make every first-run and replay entry follow the designed seven-stage walkthrough, keep progress moving when a step is skipped, and apply the newly confirmed persistence rules for guest and Journal data.

## Done looks like
- Choosing “Sign up later” enters guest mode and starts the same seven-stage walkthrough instead of dropping directly into the app.
- “Skip” advances to the next tooltip, field, or stage; it never exits the entire walkthrough unless the user explicitly ends it.
- The walkthrough covers add plant, confirmation, seat guidance, Conservatory search, Journal privacy, Herbarium Species, and return to Greenhouse in order.
- The notes field gets its own final plant-setup tooltip with examples such as pests, dying leaves, crisping tips, and brown edges.
- Stage 4 explains: “We filled in the common name ‘pancake plant’ for you, so you can see the match!” and the search actually resolves against the correct catalog/add surface.
- Walkthrough cards never cover “Begin tending” or another required action on small iPhones.
- A first-run plant is added for both signed-in users and guests. Guest data stays on that device and the completion screen clearly explains that an account is required for cloud backup.
- A replay Journal entry can be explicitly saved permanently: guests save locally with the same cloud-backup warning, and signed-in users save normally.
- Logged-in daily Journal check-ins award points once per local day.

## Out of scope
- Saving all replay actions; the Journal save is the only explicit replay persistence exception.
- Replacing Clerk authentication.
- Adding new walkthrough stages beyond the requested seven.

## Steps
1. **Unify all first-run entry paths** -- Route signed-in and “Sign up later” users through one deterministic welcome, transparency, and walkthrough state machine.
2. **Make skipping granular** -- Track field and stage progress so Skip advances one unit and only an explicit exit abandons the walkthrough.
3. **Repair stage handoffs** -- Preserve the chosen plant and common-name search context through seat guidance, Conservatory, Journal, Herbarium, and Greenhouse completion.
4. **Add contextual guidance** -- Add the requested notes tooltip and responsive placement that never blocks the action it describes.
5. **Apply persistence rules** -- Save first-run plants normally, add the optional Journal promotion from practice to permanent data, and show accurate local-only/cloud-backup messaging to guests.
6. **Protect points and privacy** -- Award signed-in daily check-in points once per local day and keep every other replay mutation blocked.
7. **Exercise the rendered flow** -- Add native-oriented walkthrough checks for fresh guest, fresh signed-in, stage skipping, full completion, replay, and narrow-screen behavior.

## Relevant files
- `artifacts/mobile/app/_layout.tsx`
- `artifacts/mobile/app/(auth)/sign-in.tsx`
- `artifacts/mobile/app/add-plant.tsx`
- `artifacts/mobile/app/(tabs)/discover.tsx`
- `artifacts/mobile/app/(tabs)/garden.tsx`
- `artifacts/mobile/app/(tabs)/journal.tsx`
- `artifacts/mobile/app/(tabs)/index.tsx`
- `artifacts/mobile/components/GuidedFieldPointer.tsx`
- `artifacts/mobile/components/GuidedWalkthroughCard.tsx`
- `artifacts/mobile/components/WelcomeScreen.tsx`
- `artifacts/mobile/context/GardenContext.tsx`
- `artifacts/mobile/hooks/useJournal.ts`
- `artifacts/mobile/lib/guidedTour.ts`

---

#### #268 — Make Profile and garden controls trustworthy
- **Status:** MERGED
- **Created:** 2026-08-29T13:27:56.886Z
- **Updated:** 2026-08-29T14:12:58.556Z
- **Artifact kinds:** mobile

# Make Garden Controls Trustworthy

## What & Why
Make Profile, personalization, friends, reminders, informational copy, and date displays behave exactly as their labels promise.

## Done looks like
- Saving a name immediately replaces “Your Name,” shows “Hi, [saved name]” in the appropriate greeting, and persists/syncs consistently.
- The plant-sway control is retained only if it produces a clear visible plant-motion effect; it is accurately labeled and respects reduced-motion behavior.
- Garden Friends on shows every enabled greenhouse companion with its normal movement and activity. Garden Friends off hides all greenhouse creatures and stops all companion actions and animation work.
- On first download, a gentle-reminders explanation asks permission and offers to take the user directly to the Reminders settings section.
- The deep link scrolls to the reminder toggle and time picker; notifications occur only at the selected hour in the device’s current local timezone.
- Notification schedules are reconciled after permission, time, timezone, app-resume, and synced-preference changes without duplicate reminders.
- Garden Points copy exactly includes watering, fertilizing, Journal entries, daily check-ins, new species, permanent/free points, companions, skins, milestones, and “No payment required!”
- Greenhouse Tiers copy accurately explains plant slots, visual upgrades, exclusive companions, premium features, and special themed events.
- User-visible dates use MM-DD without a year, while internal storage retains safe full timestamps for calculations.

## Out of scope
- Changing RevenueCat tier authority, prices, or entitlements.
- Making Garden Friends visibility override individual locked/unlocked rules.
- Replacing native notification scheduling with a remote push service.

## Steps
1. **Propagate the saved name** -- Make the saved value the single personalization source for Profile and greeting surfaces.
2. **Correct visual controls** -- Verify plant sway visibly works and change Garden Friends from animation-only gating to complete visibility and activity gating.
3. **Add reminder onboarding** -- Introduce the first-download explanation, permission flow, and scroll-to-settings handoff.
4. **Make scheduling local-time exact** -- Reconcile native schedules at the chosen local hour and refresh them when device or synced settings change.
5. **Update explanatory copy** -- Apply the approved Garden Points and Greenhouse Tiers wording.
6. **Centralize date display** -- Format user-facing dates as MM-DD without weakening internal calendar and timezone calculations.
7. **Add focused checks** -- Cover name persistence, all-friends off/on behavior, reminder permission and scheduling, timezone changes, copy, and date formatting.

## Relevant files
- `artifacts/mobile/app/settings.tsx`
- `artifacts/mobile/app/(tabs)/profile.tsx`
- `artifacts/mobile/app/(tabs)/index.tsx`
- `artifacts/mobile/components/CottageCritters.tsx`
- `artifacts/mobile/components/CottageVisualizer.tsx`
- `artifacts/mobile/components/FriendIllustration.tsx`
- `artifacts/mobile/components/CompanionPose.tsx`
- `artifacts/mobile/context/GardenContext.tsx`
- `artifacts/mobile/lib/plantCareSchedule.ts`
- `artifacts/mobile/utils/notifications.ts`

---

#### #269 — Source and verify all 185 plant guides
- **Status:** MERGED
- **Created:** 2026-08-29T13:27:56.886Z
- **Updated:** 2026-08-29T14:50:34.643Z

# Source Published Plant Guides

## What & Why
Research and visibly source every one of the 185 published Herbarium guides so care guidance is trustworthy, auditable, and corrected where current data conflicts with reliable references.

## Done looks like
- Every published plant has explicit, clickable sources near the top of its individual Herbarium guide.
- Coverage is exactly 185/185; unpublished working records are not included.
- Scientific name, family, light, watering, temperature, toxicity, and other safety-sensitive guidance are checked against authoritative references.
- Cultivars and ambiguous common names are mapped carefully instead of silently inheriting unsupported species-level claims.
- Conflicts and estimated care intervals are handled transparently rather than presented as false precision.
- The existing root-space nudge remains gentle and only appears when repeated early watering suggests the pot may be undersized; its wording and trigger are reviewed against the gathered horticultural evidence.
- Automated validation rejects missing/extra source records, malformed links, accidental source omission, and incomplete published coverage.

## Out of scope
- Researching the 1,054 unpublished working records.
- Diagnosing root binding from a photo or claiming certainty from watering frequency alone.
- Adding new plant species.

## Steps
1. **Establish the authoritative 185 list** -- Join the published IDs to the API/mobile records and create a review ledger for every species and cultivar.
2. **Research each guide** -- Use Kew POWO for taxonomy, RHS or university extension sources for cultivation, ASPCA or equivalent veterinary authority for toxicity, and specialist authorities where needed.
3. **Fact-check current guidance** -- Correct unsupported or conflicting high-value fields while preserving documented estimates and cultivar uncertainty.
4. **Publish per-plant evidence** -- Attach reviewed sources to every record and render them prominently near the top of the individual guide.
5. **Validate the root-space heuristic** -- Keep the existing repeated-early-watering signal conservative, improve its explanation, and ensure changing pot size resets it correctly.
6. **Enforce completeness** -- Add deterministic validation and API/UI checks for exactly 185 sourced published guides.

## Relevant files
- `artifacts/api-server/src/data/published-plant-ids.ts`
- `artifacts/api-server/src/data/plant-care-sources.ts`
- `artifacts/api-server/src/data/plants.json`
- `artifacts/api-server/src/routes/plants.ts`
- `artifacts/mobile/components/PlantPreviewSheet.tsx`
- `artifacts/mobile/data/plants.ts`
- `artifacts/mobile/data/sources.ts`
- `artifacts/mobile/app/plant/[id].tsx`
- `artifacts/mobile/context/GardenContext.tsx`
- `research/mosslight-plant-integrity-notes.md`
- `docs/mosslight-field-guide-fact-check-notes.md`

---

#### #271 — Compile downloadable updates since yesterday
- **Status:** MERGED
- **Created:** 2026-08-29T13:56:02.992Z
- **Updated:** 2026-08-29T14:04:39.860Z

# Compile New Updates List

## What & Why
Create a downloadable plain-text delta report containing the work added after the previous extensive changelog and related project-audit request on August 28, 2026. The report should distinguish shipped/verified changes, current unreleased code, newly tracked work, and routine repository noise so the owner has a clear update list rather than another duplicate full-history changelog.

## Done looks like
- A downloadable file `docs/mosslight-updates-since-2026-08-28.txt` exists in the repository.
- The file states its cutoff and coverage window, uses plain text with clear headings, and is easy to save or share.
- It covers the substantive August 29 updates: server-side account-deletion hardening and cleanup ordering, native ZIP data export, RevenueCat/entitlement and milestone reliability, profile-route recovery, deployment/runtime packaging fixes, build/release workflow safeguards, and the latest walkthrough/plant-addition work.
- It records the four current project surfaces/artifacts and identifies which changes are native-only, browser/API-related, or release tooling.
- It includes a clearly labeled “Newly tracked / not shipped” section for the seven-stage walkthrough work and the newly queued Profile controls, 185-guide research, and Apple sign-in/TestFlight work; it does not present proposed or pending work as complete.
- Duplicate commit pairs, generated assets, routine metadata, and unrelated personal-document updates are collapsed or omitted while preserving any user-visible or release-significant change.
- Each entry has a concise owner-facing summary and a status such as verified, current but unreleased, active, or queued, with source pointers to the existing changelog/audit and relevant task refs where useful.

## Out of scope
- Rewriting the complete historical changelog.
- Implementing or changing any MossLight feature, API behavior, release configuration, or connected service.
- Starting an EAS build, TestFlight upload, App Store submission, or deployment.
- Treating browser preview rendering as proof of physical-device behavior.
- Including secrets, credentials, tokens, or private document content.

## Steps
1. **Establish the comparison baseline** — Use the August 28 extensive development changelog, project status audit, existing release files, task history, and the repository history to define what was already documented and avoid repeating it.
2. **Collect and classify the delta** — Review substantive changes after the cutoff and group them into shipped/verified, current code not yet released, newly tracked work, and release/tooling notes; collapse duplicate commit pairs and routine generated-file noise.
3. **Write the downloadable report** — Produce the plain-text report with a coverage note, newest-first sections, concise summaries, explicit status labels, and a short source/index section pointing to the relevant existing records.
4. **Check completeness and accuracy** — Confirm the report includes the account deletion/export, entitlement, profile API, deployment, artifact, and walkthrough updates while clearly separating active or queued work from completed changes and omitting secrets.

## Relevant files
- `docs/mosslight-complete-development-changelog.md`
- `docs/mosslight-project-status-audit-2026-08-29.md`
- `CHANGELOG.txt`
- `CHANGELOG.md`
- `.local/tasks/full-project-changelog.md`
- `.local/tasks/repair-complete-walkthrough.md`
- `.local/tasks/make-profile-garden-controls-trustworthy.md`
- `.local/tasks/source-published-plant-guides.md`
- `.local/tasks/restore-apple-sign-in.md`
- `artifacts/mobile/utils/exportGarden.ts`
- `artifacts/mobile/context/GardenContext.tsx`
- `artifacts/mobile/lib/revenuecat.tsx`
- `artifacts/api-server/src/routes/profile.ts`
- `artifacts/api-server/src/app.ts`
- `artifacts/mobile/scripts/build.js`

---

#### #272 — Create MossLight brand reference
- **Status:** MERGED
- **Created:** 2026-08-29T14:07:49.930Z
- **Updated:** 2026-08-29T14:14:28.255Z

# Create MossLight Brand Reference

## What & Why
Create a downloadable, source-audited brand reference for both the native MossLight app and the MossLight Web Preview. It should give the owner the exact UI color codes, the font families and weights actually used, the user-facing screen/title locations for those fonts, and the applicable font/icon licensing plus current version/build metadata.

## Done looks like
- A downloadable plain-text document exists at `docs/mosslight-brand-reference.txt` and is easy to save or share.
- The document has clearly separated sections for `artifacts/mobile` (native app) and `artifacts/mosslight-preview` (browser preview), with an audit/snapshot note.
- Native colors include the authoritative Quiet Grove brand tokens and every selectable light/dark/accessibility theme token, with token names and exact six-digit hex values; intentional UI-specific literal colors are listed separately from the theme matrix. Plant/image artwork pixels and incidental third-party/generated utility styles are excluded.
- Preview colors include the light and dark theme values converted from the source HSL tokens to hex, plus intentional screen-specific literal colors that are not represented by theme tokens. Any non-theme defaults or hard-coded not-found/guide colors are clearly labeled.
- The font inventory names every rendered family, variant, and weight: native Raleway, Playfair Display, Cormorant Garamond, Great Vibes, and the Ionicons icon font; preview Fraunces, Outfit, and the Menlo/system fallback. It separately calls out references that are linked or declared but not currently rendered/loaded, such as preview Inter and the preview Clerk Raleway declaration, rather than presenting them as confirmed active fonts.
- Font usage is mapped using only user-facing screen names/titles (not source file paths in the usage column). Native route titles include the Greenhouse, Conservatory, Journal, Herbarium, Profile, Settings, What's New, sign-in, add/import, plant/species detail, and fallback surfaces; preview mappings distinguish currently routed titles from source-only Journal/Profile pages.
- Each bundled Google font has a precise SIL Open Font License 1.1 / free-use-with-OFL-conditions note and an official source/license reference. Ionicons is identified with its applicable MIT license, and system fallbacks are marked as platform-provided rather than incorrectly labeled OFL.
- Version/build metadata reflects the current source: native app version `2.1.0` and iOS build number `15`, plus the native artifact metadata version; preview package/artifact versions are recorded with an explicit “no separate build number found” note. Stale historical build values are not substituted for current source values.
- The document explicitly states that this is an inventory of the current source and does not represent a new EAS build, TestFlight upload, deployment, or physical-device verification.

## Out of scope
- Changing colors, typography, screen titles, fonts, dependencies, or licensing in either product.
- Creating a new design system, redesigning the app, or documenting image/artwork pixel colors.
- Starting an EAS build, TestFlight/App Store submission, web deployment, or connected-service operation.
- Making legal conclusions beyond reporting the applicable upstream license text and source links.

## Steps
1. **Audit authoritative sources** — Trace the native app’s loaded fonts, typography aliases, brand palette, theme registry, screen routes/titles, and version/build configuration; trace the preview’s loaded/linked fonts, CSS theme tokens, screen routes/titles, literal colors, and package/artifact metadata. Distinguish active rendering from unused or merely declared references.
2. **Normalize the color inventory** — Convert preview HSL tokens to exact hex values and organize both surfaces into readable token tables. Include every native selectable theme and intentional hard-coded UI accents, while collapsing duplicates only when the token-to-value relationship remains clear.
3. **Build the screen/title font map** — Group font families and variants by the user-facing screen name/title where they appear, using dynamic titles where the product generates them. Mark native shared overlays and preview source-only pages separately so the document does not imply that an unreachable preview route is active.
4. **Verify licensing and release metadata** — Check the upstream Google Fonts/OFL, Ionicons/MIT, and system-font status; record official references and avoid unsupported “free” claims. Reconcile app version/build values against current configuration and label missing preview build metadata explicitly.
5. **Write and validate the downloadable document** — Produce the plain-text file with a contents-style heading structure, readable tables, exact hex codes, license/version sections, source pointers, and a final limitations note. Check that all requested app and preview surfaces are represented, no secrets are copied, and no historical audit value is mistaken for the current build.

## Relevant files
- `artifacts/mobile/constants/brand.ts`
- `artifacts/mobile/constants/colors.ts`
- `artifacts/mobile/constants/themes.ts`
- `artifacts/mobile/constants/typography.ts`
- `artifacts/mobile/app/_layout.tsx`
- `artifacts/mobile/app/(tabs)/_layout.tsx`
- `artifacts/mobile/app.config.ts`
- `artifacts/mobile/package.json`
- `artifacts/mobile/.replit-artifact/artifact.toml`
- `artifacts/mosslight-preview/src/index.css`
- `artifacts/mosslight-preview/src/App.tsx`
- `artifacts/mosslight-preview/src/components/navigation.tsx`
- `artifacts/mosslight-preview/src/pages/home.tsx`
- `artifacts/mosslight-preview/src/pages/plants.tsx`
- `artifacts/mosslight-preview/src/pages/plant-detail.tsx`
- `artifacts/mosslight-preview/src/pages/learn.tsx`
- `artifacts/mosslight-preview/src/pages/garden.tsx`
- `artifacts/mosslight-preview/src/pages/journal.tsx`
- `artifacts/mosslight-preview/src/pages/profile.tsx`
- `artifacts/mosslight-preview/src/pages/sign-in.tsx`
- `artifacts/mosslight-preview/src/pages/sign-up.tsx`
- `artifacts/mosslight-preview/src/pages/not-found.tsx`
- `artifacts/mosslight-preview/package.json`
- `artifacts/mosslight-preview/.replit-artifact/artifact.toml`
- `artifacts/mosslight-preview/index.html`

---

#### #285 — Revamp the mobile paywall
- **Status:** MERGED
- **Created:** 2026-08-29T19:03:00.000Z
- **Updated:** 2026-08-29T19:41:24.000Z
- **Artifact kinds:** mobile

# Revamp Mobile Paywall

## What & Why
Rebuild the MossLight upgrade and skin purchase sheet so it fits safely around the iPhone Dynamic Island, shows actual products immediately, and accurately represents the expanded companion catalog. The former composition clipped tier markers, crowded the brand title into the safe area, pushed products below marketing copy, and maintained a stale four-skin catalog with an inaccurate animation promise.

## Done looks like
- “Cultivate Wonder” and the close control sit below the top safe-area inset; the full-screen sheet also respects the bottom inset.
- Clipped decorative tier dots are removed and tier progression is represented by understandable product cards.
- Tier and companion products appear directly beneath the tabs, with a compact supporting summary.
- The companion inventory is derived from canonical `SKIN_UNLOCKS` metadata and canonical preview assets, including conservation skins.
- Cards identify selected, active/equipped, owned, available, loading, unavailable, conservation, and Utopia+ visit states with text and icons rather than color alone.
- The inaccurate claim that every companion has animations is removed.
- Purchase actions are only enabled when a RevenueCat package is available; test-store confirmation and existing success flow remain intact.
- The layout uses semantic theme colors and has an Extra Large text path that keeps the tab control readable.

## Out of scope
- Creating new RevenueCat products or changing App Store pricing.
- Changing entitlement authority, account identity handling, or subscription persistence.
- Adding new companion artwork or movement behavior.
- Changing the Profile or Conservation catalog outside the shared canonical metadata already used by the paywall.

## Implementation notes
- `PaywallModal` now uses a safe-area-aware full-screen composition with a compact header, tabs, product-first cards, and a footer in normal layout flow.
- Tier copy is derived from `TIER_INFO`; companion cards are derived from `SKIN_UNLOCKS` and `getSkinPreviewSource`, with conservation membership based on the canonical conservation skin IDs.
- RevenueCat lookup methods, package identifiers, prices, confirmation behavior, restoration, and post-purchase callbacks were not changed.
- The browser screenshot check could not load authenticated app state because the existing Expo-to-API cross-origin configuration returned a CORS error; Metro bundled successfully and the mobile validation suite passed.

## Verification
- Mobile typecheck passed.
- Learn catalog, web profile-sync, plant-asset, and critical-flow validations passed.
- `git diff --check` passed.
- Expo restarted and Metro bundled successfully.

## Relevant files
- `artifacts/mobile/components/PaywallModal.tsx`
- `artifacts/mobile/context/GardenContext.tsx`
- `artifacts/mobile/lib/critterSkins.ts`
- `artifacts/mobile/lib/revenuecat.tsx`
- `artifacts/mobile/constants/themes.ts`
- `artifacts/mobile/hooks/useColors.ts`

---

### CANCELLED (67)

#### #27 — Keep guest mode working reliably when the device is offline
- **Status:** CANCELLED
- **Created:** 2026-05-22T14:17:52.110Z
- **Updated:** 2026-05-22T14:22:04.914Z
- **Depends on:** #18
- **Proposed from:** #18
- **Category:** next_steps

# Keep guest mode working reliably when the device is offline

  ## What & Why
  Guest mode flag is now stored in `GardenContext` and read from AsyncStorage once at startup. However, if AsyncStorage itself fails (rare but possible on low-memory devices or corrupted storage), `isGuest` defaults to `false`, which would redirect a guest user to sign-in. Adding a fallback (e.g. an in-memory flag set synchronously when the user taps "Continue as guest") would guarantee guests never get bounced unexpectedly.

  ## Done looks like
  - A secondary in-memory fallback for guest state that survives the current JS session (e.g. a module-level variable or a ref set immediately in `setIsGuest(true)`)
  - Guest users are never redirected to sign-in within the same session, even if AsyncStorage read fails on startup

  ## Relevant files
  - `artifacts/mobile/context/GardenContext.tsx` (setIsGuest, isGuest state)
  - `artifacts/mobile/app/(auth)/sign-in.tsx` (handleSkip sets guest mode)
  - `artifacts/mobile/app/(tabs)/_layout.tsx` (redirect guard)

---

#### #28 — Clear guest mode automatically when a guest successfully signs in
- **Status:** CANCELLED
- **Created:** 2026-05-22T14:17:52.110Z
- **Updated:** 2026-05-22T14:22:04.914Z
- **Depends on:** #18
- **Proposed from:** #18
- **Category:** incomplete_scope

# Clear guest mode automatically when a guest successfully signs in

  ## What & Why
  `AppShell` in `app/_layout.tsx` clears guest mode via `setIsGuest(false)` only in the welcome-gate effect. The SSO sign-in flow in `sign-in.tsx` does not call `setIsGuest(false)` after a successful sign-in, relying instead on the AppShell effect. If the welcome screen is skipped (returning user), the guest flag might linger in context until the next cold start.

  ## Done looks like
  - After a successful SSO sign-in in `handleSignIn` (sign-in.tsx), `setIsGuest(false)` is called explicitly
  - Guest context state is always cleared as part of the sign-in success path, not deferred to an effect

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx` (handleSignIn callback, ~line 140–220)
  - `artifacts/mobile/app/_layout.tsx` (AppShell effect, ~line 161–185)
  - `artifacts/mobile/context/GardenContext.tsx` (setIsGuest)

---

#### #34 — Let users see a preview before replacing their local plants with the cloud garden
- **Status:** CANCELLED
- **Created:** 2026-05-22T14:57:51.318Z
- **Updated:** 2026-05-22T15:02:34.097Z
- **Depends on:** #22
- **Proposed from:** #22
- **Category:** next_steps

# Let users see a preview before replacing their local plants with the cloud garden

  ## What & Why
  The "Use my cloud garden" option in the two-gardens conflict dialog immediately replaces all local plants with the cloud version — no preview, no way to see what you're losing. Now that merge has a preview, this option stands out as the one that still acts immediately.

  ## Done looks like
  - Tapping "Use my cloud garden" opens a preview sheet (similar to MergePreviewSheet) showing which local plants will be lost and what the cloud garden contains
  - Users can confirm or go back
  - Consistent with the new merge preview pattern

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx` — the "Use my cloud garden" handler
  - `artifacts/mobile/components/MergePreviewSheet.tsx` — reference for the existing preview sheet pattern

---

#### #35 — Show plant details (species, location) in the merge preview
- **Status:** CANCELLED
- **Created:** 2026-05-22T14:57:51.318Z
- **Updated:** 2026-05-22T15:02:34.097Z
- **Depends on:** #22
- **Proposed from:** #22
- **Category:** next_steps

# Show plant details (species, location) in the merge preview

  ## What & Why
  The merge preview currently lists plants by nickname only. Users with multiple plants of the same species can't easily tell them apart without seeing the location or species name alongside.

  ## Done looks like
  - Each plant row in the merge preview shows nickname + location or species name as a secondary line
  - Keeps the sheet readable without being cluttered

  ## Relevant files
  - `artifacts/mobile/components/MergePreviewSheet.tsx` — PlantRow and Section components

---

#### #36 — Let users go back to the 'two gardens found' screen after tapping 'Use my cloud garden'
- **Status:** CANCELLED
- **Created:** 2026-05-22T15:01:58.364Z
- **Updated:** 2026-05-22T15:03:57.536Z
- **Depends on:** #23
- **Proposed from:** #23
- **Category:** incomplete_scope

# Let users go back to the 'two gardens found' screen after tapping 'Use my cloud garden'

  ## What & Why
  The current "Go back" button in the second confirmation alert dismisses the alert but leaves the user at the top level of sign-in with no way to reconsider the other options (merge, keep local plants). The "Two gardens found" alert should reappear.

  ## Done looks like
  - Tapping "Go back" on the "Remove local plants?" confirmation re-shows the "Two gardens found" alert
  - All three options (merge, keep, use cloud) are accessible again

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx` — the two-garden Alert and the new confirmation alert

---

#### #37 — Keep the conflict dialog available if the user dismisses it by accident
- **Status:** CANCELLED
- **Created:** 2026-05-22T15:06:07.619Z
- **Updated:** 2026-05-22T15:13:43.801Z
- **Depends on:** #24
- **Proposed from:** #24
- **Category:** next_steps

# Keep the conflict dialog available if the user dismisses it by accident

  ## What & Why
  The "Two gardens found" Alert is not cancelable (cancelable: false), but if the user somehow exits the flow without picking an option (e.g. via a deep-link interrupt or app backgrounding), `syncPausedRef` stays `true` permanently and the background sync never resumes. There's no recovery path short of restarting the app.

  ## Done looks like
  - If the app returns to foreground with `syncPausedRef.current === true` but no dialog is visible, resume sync automatically (e.g. via an AppState listener or a timeout safety valve)
  - Alternatively, persist the conflict state across app restarts so the dialog can be re-shown on next launch

  ## Relevant files
  - `artifacts/mobile/context/GardenContext.tsx` — syncPausedRef, pauseSync/resumeSync
  - `artifacts/mobile/app/(auth)/sign-in.tsx` — conflict resolution flow

---

#### #38 — Prevent stale data from landing when the app wakes up mid-sync on a second device
- **Status:** CANCELLED
- **Created:** 2026-05-22T15:06:07.619Z
- **Updated:** 2026-05-22T15:13:43.801Z
- **Depends on:** #24
- **Proposed from:** #24
- **Category:** next_steps

# Prevent stale data from landing when the app wakes up mid-sync on a second device

  ## What & Why
  The current debounce guards against the conflict dialog race, but there's a second window: if Device A is signed in and backgrounded, then Device B pushes a fresh snapshot, Device A can still overwrite it on next foreground resume because the debounce fires on the next state-change (e.g. a watering).  An ETag / last-write-wins strategy or a server-side conflict check (compare snapshot timestamps before accepting a push) would close this gap.

  ## Done looks like
  - The API server's push endpoint accepts an optional `ifNewerThan` timestamp; it rejects the push if the stored snapshot is already newer
  - The mobile client sends its local snapshot timestamp and handles a 409 conflict response gracefully (re-fetches and re-shows the conflict UI)

  ## Relevant files
  - `artifacts/api-server/` — profile push endpoint
  - `artifacts/mobile/utils/api.ts` — pushProfile
  - `artifacts/mobile/context/GardenContext.tsx` — debounced sync

---

#### #42 — Apply a matching slide-up entrance to the main tab screen after sign-in
- **Status:** CANCELLED
- **Created:** 2026-05-22T15:32:47.300Z
- **Updated:** 2026-05-22T16:21:12.938Z
- **Depends on:** #29
- **Proposed from:** #29
- **Category:** next_steps

# Apply a matching slide-up entrance to the main tab screen after sign-in

  ## What & Why
  The sign-in screen now has a polished slide-up entrance when the loading overlay fades. The main tab screen (shown after sign-in completes) appears abruptly with no matching entrance animation. Adding a slide-up there would give the whole launch sequence a consistent, intentional feel.

  ## Done looks like
  - After signing in (or continuing as guest), the tab screen slides up + fades in
  - The timing and easing matches the sign-in entrance animation
  - Works on iOS, Android, and web

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — entry point for the main tab navigator
  - `artifacts/mobile/app/(auth)/_layout.tsx` — reference for the existing slide-up implementation (delay: 220ms, duration: 540ms, Easing.out(Easing.cubic))

---

#### #43 — Stagger the loading dots entrance to match the logo fade-in
- **Status:** CANCELLED
- **Created:** 2026-05-22T15:42:24.068Z
- **Updated:** 2026-05-22T16:27:09.854Z
- **Depends on:** #30
- **Proposed from:** #30
- **Category:** next_steps

# Stagger the loading dots entrance to match the logo fade-in

  ## What & Why
  The logo now fades in on mount, but the three pulsing dots beneath it appear instantly with no entrance. Adding a short delayed fade-in to the dots (starting after the logo finishes its ~400ms entrance) would make the full loading screen feel cohesive and intentional.

  ## Done looks like
  - The dots row fades in after the logo entrance completes (~400ms delay)
  - The entrance is subtle (~300ms) so it doesn't slow perceived load
  - Works on iOS, Android, and web

  ## Relevant files
  - `artifacts/mobile/components/BrandedLoadingScreen.tsx`

---

#### #54 — Make the sign-in screen feel more alive with a gentle entrance animation
- **Status:** CANCELLED
- **Created:** 2026-05-22T19:35:07.771Z
- **Updated:** 2026-05-22T19:40:17.233Z
- **Depends on:** #53
- **Proposed from:** #53
- **Category:** next_steps

# Make the sign-in screen feel more alive with a gentle entrance animation

  ## What & Why
  The sign-in screen is the first thing new users see. A subtle fade-in or slide-up entrance for the logo, heading, and buttons would make the first impression more polished and app-like — especially now that the layout is responsive across all screen sizes.

  ## Done looks like
  - The MossLight logo, heading, and buttons animate in smoothly when the screen mounts
  - Animation is lightweight and respects reduced-motion preferences
  - Works well on both small (SE/mini) and standard screens

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx`

---

#### #57 — Show plant names or species icons in the sign-in garden preview
- **Status:** CANCELLED
- **Created:** 2026-05-22T19:37:11.035Z
- **Updated:** 2026-05-22T19:41:21.659Z
- **Depends on:** #55
- **Proposed from:** #55
- **Category:** next_steps

# Show plant names or species icons in the sign-in garden preview

  ## What & Why
  The current preview shows a count ("3 plants ready to back up"), but showing a few plant names or emoji icons would make the preview feel personal and increase the emotional pull to sign in and keep those specific plants safe.

  ## Done looks like
  - Up to 2–3 plant names or species emoji are shown inside the badge (e.g. "Monstera, Pothos + 1 more")
  - Falls back gracefully to the count-only label when there are many plants

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx`
  - `artifacts/mobile/context/GardenContext.tsx`
  - `artifacts/mobile/data/plants.ts` (for species emoji/icon lookup)

---

#### #58 — Animate the sign-in buttons and text onto the screen
- **Status:** CANCELLED
- **Created:** 2026-05-22T19:41:15.815Z
- **Updated:** 2026-05-22T19:45:50.354Z
- **Depends on:** #56
- **Proposed from:** #56
- **Category:** next_steps

# Animate the sign-in buttons and text onto the screen

  ## What & Why
  The garden preview badge now has a polished fade/slide-up entrance, but the rest of the sign-in screen (title, heading, subtitle, auth buttons) still appears instantly. Staggered entrance animations for the hero content would complete the screen's motion story.

  ## Done looks like
  - The title, heading, sub-copy, and buttons each fade or slide in sequentially on mount
  - Timing is staggered so content cascades in naturally, not all at once
  - Animation respects the app's existing subtle motion style

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx`

---

#### #59 — Let users tap 'Try again' to actually retry connecting, not just reset the timer
- **Status:** CANCELLED
- **Created:** 2026-05-22T19:57:02.882Z
- **Updated:** 2026-05-22T19:58:45.101Z
- **Depends on:** #45
- **Proposed from:** #45
- **Category:** next_steps

# Let users tap 'Try again' to actually retry connecting, not just reset the timer

  ## What & Why
  The current 'Try again' button on the loading screen resets the visual timers but doesn't trigger a real reconnect attempt — Clerk and the garden context are still waiting on the same stalled promises. A true retry should re-trigger auth and data initialization so users have a real path out of a stuck state.

  ## Done looks like
  - BrandedLoadingScreen accepts an optional `onRetry` prop
  - Callers in `artifacts/mobile/app/(auth)/_layout.tsx` and `artifacts/mobile/app/(tabs)/_layout.tsx` pass a handler that re-triggers Clerk reload or garden context refresh
  - Tapping 'Try again' produces an observable effect (spinner resets AND a new network request is made)

  ## Relevant files
  - `artifacts/mobile/components/BrandedLoadingScreen.tsx`
  - `artifacts/mobile/app/(auth)/_layout.tsx`
  - `artifacts/mobile/app/(tabs)/_layout.tsx`

---

#### #60 — Show a banner when the app goes offline mid-session
- **Status:** CANCELLED
- **Created:** 2026-05-22T19:57:02.882Z
- **Updated:** 2026-05-22T19:58:45.101Z
- **Depends on:** #45
- **Proposed from:** #45
- **Category:** next_steps

# Show a banner when the app goes offline mid-session

  ## What & Why
  The loading screen now surfaces a connection error on startup, but users who lose connectivity after the app has loaded get no feedback — actions silently fail. A lightweight offline banner would make the app feel more reliable.

  ## Done looks like
  - A non-intrusive banner slides in at the top whenever the device loses network connectivity (using NetInfo)
  - The banner disappears automatically when the connection is restored
  - The existing loading screen error state is unaffected

  ## Relevant files
  - `artifacts/mobile/components/` — add a new `OfflineBanner.tsx` component
  - `artifacts/mobile/app/_layout.tsx` — mount the banner at the root layout level

---

#### #61 — Add a gentle bounce or spring feel to the tab entrance animation
- **Status:** CANCELLED
- **Created:** 2026-05-22T20:03:05.723Z
- **Updated:** 2026-05-22T20:09:40.277Z
- **Depends on:** #44
- **Proposed from:** #44
- **Category:** next_steps

# Add a gentle bounce or spring feel to the tab entrance animation

  ## What & Why
  The current tab entrance animation fades and slides up with a simple ease curve. Using a spring or overshoot curve would make the motion feel more natural and playful, matching the cozy, botanical vibe of the app.

  ## Done looks like
  - The entrance animation uses a spring easing (slight overshoot) instead of a plain ease-out
  - The feel is snappy but not distracting — the effect is subtle enough to feel intentional rather than bouncy

  ## Relevant files
  - `artifacts/mobile/hooks/useTabEntranceAnimation.ts`

---

#### #62 — Animate screens inside nested stacks the same way as tabs
- **Status:** CANCELLED
- **Created:** 2026-05-22T20:03:05.723Z
- **Updated:** 2026-05-22T20:09:40.277Z
- **Depends on:** #44
- **Proposed from:** #44
- **Category:** next_steps

# Animate screens inside nested stacks the same way as tabs

  ## What & Why
  The tab screens now animate in on first visit, but deeper screens (plant detail, add-plant, settings, etc.) still use the default navigation transition. Bringing the same fade-up entrance to those screens would keep the motion language consistent throughout the app.

  ## Done looks like
  - Screens that push onto a stack (e.g. plant detail, add-plant flow) animate in with the same fade + slide-up pattern used for tabs
  - The hook from `useTabEntranceAnimation` is reused or adapted for push screens

  ## Relevant files
  - `artifacts/mobile/hooks/useTabEntranceAnimation.ts`
  - `artifacts/mobile/app/(tabs)/index.tsx` (push to detail screens)
  - Stack screens under `artifacts/mobile/app/`

---

#### #63 — Show the countdown timer also for 'replaced by cloud' banner
- **Status:** CANCELLED
- **Created:** 2026-05-22T20:11:26.628Z
- **Updated:** 2026-05-22T20:46:06.966Z
- **Depends on:** #47
- **Proposed from:** #47
- **Category:** incomplete_scope

# Show the countdown timer also for 'replaced by cloud' banner

  ## What & Why
  The progress bar added in task #47 only animates for the 'local plants removed' (discard) banner, because the 'replaced by cloud' banner has no auto-dismiss timer. Adding an equivalent 60-second auto-dismiss (with progress bar) to the replaced-by-cloud case gives both banners consistent, predictable behavior.

  ## Done looks like
  - `replacedByCloudSnapshotTs` triggers the same 60-second auto-dismiss + progress animation as `discardSnapshotTs`
  - The bar drains identically, stops on Undo or Dismiss
  - Behavior is consistent for both banner variants

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — UndoBanner component (both useEffect blocks and the progress animation)

---

#### #64 — Let users adjust how long the undo banner stays on screen
- **Status:** CANCELLED
- **Created:** 2026-05-22T20:11:26.628Z
- **Updated:** 2026-05-22T20:46:06.966Z
- **Depends on:** #47
- **Proposed from:** #47
- **Category:** next_steps

# Let users adjust how long the undo banner stays on screen

  ## What & Why
  60 seconds is a reasonable default but some users may want more or less time. Exposing a preference (e.g. 30 / 60 / 120 seconds) in Settings gives power users control while keeping a sensible default for everyone else.

  ## Done looks like
  - A setting in the Settings screen lets users pick the undo banner duration (30s / 60s / 120s)
  - The chosen duration is persisted and respected by the progress bar and auto-dismiss timer in UndoBanner

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — BANNER_DURATION constant to wire to the setting
  - Settings screen (wherever it lives in the mobile artifact)

---

#### #65 — Extend the 'saved preference' fast path to also show the undo banner
- **Status:** CANCELLED
- **Created:** 2026-05-22T20:49:19.464Z
- **Updated:** 2026-05-23T01:55:00.781Z
- **Depends on:** #48
- **Proposed from:** #48
- **Category:** next_steps

# Extend the 'saved preference' fast path to also show the undo banner

  ## What & Why
  When a user previously chose "merge" and signed in again, the merge runs silently via the saved-preference fast path — no confirmation is shown. The undo banner now appears for this path (added in the merge-undo task), but users who set their saved preference to "keep_local" or "use_cloud" also get silent destructive actions with no undo. Extending the banner to those two saved-pref paths would give users a complete safety net for all re-login scenarios.

  ## Done looks like
  - Before "keep_local" fast path runs, a pre-discard snapshot is taken (matching the discardLocalPlants snapshot pattern)
  - Before "use_cloud" fast path runs, a pre-replace snapshot is taken
  - The undo banner appears after each fast path action, matching behavior for the non-fast-path dialogs

  ## Relevant files
  - `artifacts/mobile/app/(auth)/sign-in.tsx` — savedPref fast paths for keep_local and use_cloud (lines ~180-210)
  - `artifacts/mobile/context/GardenContext.tsx` — discardLocalPlants, restoreFromCloudSnapshot
  - `artifacts/mobile/app/(tabs)/_layout.tsx` — UndoBanner

---

#### #66 — Clear the merge undo snapshot when the user makes further changes to their garden
- **Status:** CANCELLED
- **Created:** 2026-05-22T20:49:19.464Z
- **Updated:** 2026-05-23T01:55:00.781Z
- **Depends on:** #48
- **Proposed from:** #48
- **Category:** tech_debt

# Clear the merge undo snapshot when the user makes further changes to their garden

  ## What & Why
  The pre-merge snapshot persists for up to 24 hours. If the user adds, removes, or waters plants after a merge, tapping "Undo" will restore the pre-merge state and silently discard those post-merge changes — with no warning. Clearing the snapshot automatically on the first meaningful garden edit would prevent confusing data loss.

  ## Done looks like
  - Any call to addPlant, removePlant, waterPlant, fertilizePlant, or updateNotes clears the pre-merge snapshot (removes it from AsyncStorage and sets preMergeSnapshotTs to null)
  - The undo banner disappears as soon as the user makes their first garden edit after merging

  ## Relevant files
  - `artifacts/mobile/context/GardenContext.tsx` — addPlant, removePlant, waterPlant, fertilizePlant, updateNotes, clearMergeSnapshot

---

#### #75 — Generate App Store screenshots at 2778×1284px
- **Status:** CANCELLED
- **Created:** 2026-05-28T14:45:45.013Z
- **Updated:** 2026-05-28T20:55:19.283Z
- **Artifact kinds:** design

# App Store Screenshots at 2778×1284px

## What & Why
Apple requires App Store screenshots at 2778×1284px for the 6.7" iPhone (iPhone 14 Pro Max / 15 Pro Max) display class. The team needs a set of polished screenshots at this exact size for App Store Connect submission/nomination.

## Done looks like
- A set of at least 5 screenshot images exported at exactly 2778×1284px covering the app's key screens (home garden, companions, plant detail, journal, profile/skins)
- Screenshots placed in a dedicated `artifacts/mobile/screenshots/appstore/` directory and accessible as files
- Each screenshot looks polished — correct fonts, real content (not placeholder data), and proper dark or light theme as appropriate

## Out of scope
- iPad screenshots (different dimensions)
- Animated previews or App Preview videos
- Uploading to App Store Connect directly

## Steps
1. **Build a screenshot renderer** — Create a React/web page (in the mockup-sandbox or as a small standalone Vite page) that renders each key app screen inside a fixed 2778×1284px viewport. Use the app's actual components, colors, and fonts. Seed it with representative plant/companion data so screens look populated.

2. **Define the 5 core screens** — Render: (1) Garden view with a few plants, (2) Plant detail/care card, (3) Companions panel with critters visible, (4) Field Journal, (5) Profile/Skins shop. Each should be cropped/composed to fill the full 2778×1284px frame.

3. **Export screenshots** — Use Playwright or a server-side screenshot tool to capture each rendered page at exactly 2778×1284px and save as PNG files to `artifacts/mobile/screenshots/appstore/`.

4. **Verify output** — Confirm each exported PNG is exactly 2778×1284 pixels and visually clean (no clipping, no placeholder text).

## Relevant files
- `artifacts/mobile/assets/critters/`
- `artifacts/mobile/constants/colors.ts`
- `artifacts/mobile/constants/themes.ts`
- `artifacts/mobile/context/GardenContext.tsx:170-220`
- `artifacts/mobile/app/(tabs)/index.tsx`
- `artifacts/mobile/app/(tabs)/garden.tsx`
- `artifacts/mobile/app/(tabs)/profile.tsx`
- `artifacts/mobile/app/(tabs)/journal.tsx`

---

#### #110 — Show a locked skin's description as a teaser before it's earned
- **Status:** CANCELLED
- **Created:** 2026-05-29T01:55:14.350Z
- **Updated:** 2026-05-29T02:20:19.611Z
- **Depends on:** #98
- **Proposed from:** #98
- **Category:** next_steps

# Show a locked skin's description as a teaser before it's earned

  ## What & Why
  Currently, locked free milestone skins show only "N pts to unlock" — no hint of what the companion actually is. Showing the description (slightly dimmed or with a lock icon) gives users something to look forward to and motivates earning more points.

  ## Done looks like
  - Locked free skin rows show the description text in a dimmed/muted style alongside or in place of the points hint
  - The locked state is visually distinct from the unlocked state (e.g. softer opacity, lock icon prefix)

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — free skin row rendering (lines 688–696)
  - `artifacts/mobile/context/GardenContext.tsx` — SKIN_UNLOCKS descriptions (lines 160–168)

---

#### #111 — Shared-Device Account Isolation
- **Status:** CANCELLED
- **Created:** 2026-05-29T01:56:13.896Z
- **Updated:** 2026-05-29T01:56:29.870Z

Vulnerabilities caused by device-local MossLight state surviving sign-out and being reused across later guest or authenticated sessions on the same installation.

Vulnerabilities to fix:

1. [High] Retained local and undo snapshots can be synced into a different account on the same device
  A new user who signs into the app on a shared device can accidentally import the previous user's local garden into their own cloud account. Even when they choose to use their own cloud data, the old device snapshot remains undoable and can still be pushed into the new account afterward.

The root cause is that account transitions do not clear or re-scope the device-local profile cache or the recovery snapshots used by conflict resolution. `performSignOut()` removes only the conflict-preference key and leaves the live profile keys and snapshot keys in place (`artifacts/mobile/app/(tabs)/profile.tsx:160-167`). The retained live data includes the profile snapshot keys enumerated in `ALL_PROFILE_KEYS`, such as `@quietgrove:userPlants`, `@quietgrove:userName`, `@quietgrove:points`, `@quietgrove:friendPrefs`, `@quietgrove:journalEntries`, `@quietgrove:device_id`, and `@quietgrove:greenhouse_code` (`artifacts/mobile/utils/api.ts:7-31`). The recovery features also use global, non-user-scoped keys: `@quietgrove:discarded_snapshot`, `@quietgrove:replaced_by_cloud_snapshot`, and `@quietgrove:pre_merge_snapshot` (`artifacts/mobile/context/GardenContext.tsx:298-303`).

When a different person signs in later on the same device, the sign-in flow treats the retained local plants as current device content. If `userPlants` contains any non-demo data, `handleSSO()` activates the new session, fetches that new account's cloud profile, and then offers conflict actions based on the stale local state (`artifacts/mobile/app/(auth)/sign-in.tsx:165-332`). Choosing "Keep these plants" or "Merge both gardens" immediately calls `forceSyncToCloud()` after the new Clerk session is active, so the previous user's retained local garden is uploaded under the new user's bearer token (`artifacts/mobile/app/(auth)/sign-in.tsx:207-233,274-279,526-535`; `artifacts/mobile/context/GardenContext.tsx:1132-1139`).

The supposedly safer "Use my cloud garden" path is also vulnerable. `restoreFromCloudSnapshot()` first stores the current local garden into `@quietgrove:replaced_by_cloud_snapshot`, then applies the cloud snapshot (`artifacts/mobile/context/GardenContext.tsx:1282-1323`). The tab layout always mounts an undo banner for any signed-in or guest session (`artifacts/mobile/app/(tabs)/_layout.tsx:27-45,130-179,334-377`). If the new user taps Undo, `undoReplaceByCloud()` restores that saved local snapshot and then calls `pushProfile()` with the currently signed-in user's token, overwriting the cloud profile with the previous device user's data (`artifacts/mobile/context/GardenContext.tsx:1325-1362`). The same cross-account problem exists for the other global recovery snapshots used by `discardLocalPlants()` / `undoDiscardLocalPlants()` and `snapshotPreMerge()` / `undoMerge()` (`artifacts/mobile/context/GardenContext.tsx:1142-1275`).

A practical exploit path is: Alice signs out on a shared phone without clearing local state; Bob signs in with his own account; Bob chooses either "Keep these plants" or "Merge both gardens," causing Alice's retained plants and related snapshot data to sync into Bob's server-backed profile. Even if Bob chooses "Use my cloud garden," the app preserves Alice's prior local garden in the undo snapshot, and pressing Undo within the retention window pushes Alice's data into Bob's account anyway. This creates both a confidentiality issue (Alice's data is exposed to Bob during the flow) and a server-side integrity issue (Bob's cloud profile can be overwritten with Alice's retained device state).
  Files: artifacts/mobile/app/(tabs)/profile.tsx, artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/app/(tabs)/_layout.tsx, artifacts/mobile/context/GardenContext.tsx, artifacts/mobile/utils/api.ts

2. [High] Sign-out leaves prior user's garden and journal visible to the next guest on the same device
  Signing out does not clear the previous person's locally cached garden or journal. Anyone who uses the same device afterward can tap “Sign up later” and immediately browse the prior user's plants, notes, and journal history without signing into that account.

The sign-out flow only removes the per-user conflict preference and clears guest mode before redirecting back to the sign-in screen; it does not clear the actual profile cache or journal cache (`artifacts/mobile/app/(tabs)/profile.tsx:160-167`). The shared `GardenProvider` then rehydrates plant and profile state directly from global AsyncStorage keys such as `@quietgrove:userPlants`, `@quietgrove:userName`, `@quietgrove:points`, and related profile keys on every app load regardless of which human is currently using the device (`artifacts/mobile/context/GardenContext.tsx:404-495`). Journal entries are stored separately under a single global key, `@quietgrove:journalEntries`, and are likewise loaded without any account scoping (`artifacts/mobile/hooks/useJournal.ts:12-29`).

After sign-out, the next device user does not need to authenticate to access that retained data. The sign-in screen's “Sign up later” button simply sets `isGuest` and routes into the tab UI (`artifacts/mobile/app/(auth)/sign-in.tsx:361-364`). The tab layout explicitly allows access whenever either `isSignedIn` or `isGuest` is true (`artifacts/mobile/app/(tabs)/_layout.tsx:334-377`). Once inside, multiple screens render the retained state directly, including the profile screen's stats and plant counts (`artifacts/mobile/app/(tabs)/profile.tsx:72-103,276-281`) and the journal screen, which loads and displays all stored entries (`artifacts/mobile/app/(tabs)/journal.tsx:152-156` and `44-77`).

A realistic exploit on a shared family tablet is straightforward: Alice signs out of MossLight, hands the device to Bob, and Bob taps “Sign up later.” Bob is then dropped into the prior local session with Alice's plant collection, nicknames, notes, care history-derived stats, and journal entries still present. This is a cross-user confidentiality failure in the app's stated shared-device threat model, not merely local tampering, because ordinary app navigation exposes one person's cached content to the next person using the same installation.
  Files: artifacts/mobile/app/(tabs)/profile.tsx, artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/app/(tabs)/_layout.tsx, artifacts/mobile/context/GardenContext.tsx, artifacts/mobile/hooks/useJournal.ts, artifacts/mobile/app/(tabs)/journal.tsx

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #113 — Shared-Device Account Isolation
- **Status:** CANCELLED
- **Created:** 2026-05-29T01:56:32.295Z
- **Updated:** 2026-05-29T01:56:36.620Z

Vulnerabilities caused by device-local MossLight state surviving sign-out and being reused across later guest or authenticated sessions on the same installation.

Vulnerabilities to fix:

1. [High] Retained local and undo snapshots can be synced into a different account on the same device
  A new user who signs into the app on a shared device can accidentally import the previous user's local garden into their own cloud account. Even when they choose to use their own cloud data, the old device snapshot remains undoable and can still be pushed into the new account afterward.

The root cause is that account transitions do not clear or re-scope the device-local profile cache or the recovery snapshots used by conflict resolution. `performSignOut()` removes only the conflict-preference key and leaves the live profile keys and snapshot keys in place (`artifacts/mobile/app/(tabs)/profile.tsx:160-167`). The retained live data includes the profile snapshot keys enumerated in `ALL_PROFILE_KEYS`, such as `@quietgrove:userPlants`, `@quietgrove:userName`, `@quietgrove:points`, `@quietgrove:friendPrefs`, `@quietgrove:journalEntries`, `@quietgrove:device_id`, and `@quietgrove:greenhouse_code` (`artifacts/mobile/utils/api.ts:7-31`). The recovery features also use global, non-user-scoped keys: `@quietgrove:discarded_snapshot`, `@quietgrove:replaced_by_cloud_snapshot`, and `@quietgrove:pre_merge_snapshot` (`artifacts/mobile/context/GardenContext.tsx:298-303`).

When a different person signs in later on the same device, the sign-in flow treats the retained local plants as current device content. If `userPlants` contains any non-demo data, `handleSSO()` activates the new session, fetches that new account's cloud profile, and then offers conflict actions based on the stale local state (`artifacts/mobile/app/(auth)/sign-in.tsx:165-332`). Choosing "Keep these plants" or "Merge both gardens" immediately calls `forceSyncToCloud()` after the new Clerk session is active, so the previous user's retained local garden is uploaded under the new user's bearer token (`artifacts/mobile/app/(auth)/sign-in.tsx:207-233,274-279,526-535`; `artifacts/mobile/context/GardenContext.tsx:1132-1139`).

The supposedly safer "Use my cloud garden" path is also vulnerable. `restoreFromCloudSnapshot()` first stores the current local garden into `@quietgrove:replaced_by_cloud_snapshot`, then applies the cloud snapshot (`artifacts/mobile/context/GardenContext.tsx:1282-1323`). The tab layout always mounts an undo banner for any signed-in or guest session (`artifacts/mobile/app/(tabs)/_layout.tsx:27-45,130-179,334-377`). If the new user taps Undo, `undoReplaceByCloud()` restores that saved local snapshot and then calls `pushProfile()` with the currently signed-in user's token, overwriting the cloud profile with the previous device user's data (`artifacts/mobile/context/GardenContext.tsx:1325-1362`). The same cross-account problem exists for the other global recovery snapshots used by `discardLocalPlants()` / `undoDiscardLocalPlants()` and `snapshotPreMerge()` / `undoMerge()` (`artifacts/mobile/context/GardenContext.tsx:1142-1275`).

A practical exploit path is: Alice signs out on a shared phone without clearing local state; Bob signs in with his own account; Bob chooses either "Keep these plants" or "Merge both gardens," causing Alice's retained plants and related snapshot data to sync into Bob's server-backed profile. Even if Bob chooses "Use my cloud garden," the app preserves Alice's prior local garden in the undo snapshot, and pressing Undo within the retention window pushes Alice's data into Bob's account anyway. This creates both a confidentiality issue (Alice's data is exposed to Bob during the flow) and a server-side integrity issue (Bob's cloud profile can be overwritten with Alice's retained device state).
  Files: artifacts/mobile/app/(tabs)/profile.tsx, artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/app/(tabs)/_layout.tsx, artifacts/mobile/context/GardenContext.tsx, artifacts/mobile/utils/api.ts

2. [High] Sign-out leaves prior user's garden and journal visible to the next guest on the same device
  Signing out does not clear the previous person's locally cached garden or journal. Anyone who uses the same device afterward can tap “Sign up later” and immediately browse the prior user's plants, notes, and journal history without signing into that account.

The sign-out flow only removes the per-user conflict preference and clears guest mode before redirecting back to the sign-in screen; it does not clear the actual profile cache or journal cache (`artifacts/mobile/app/(tabs)/profile.tsx:160-167`). The shared `GardenProvider` then rehydrates plant and profile state directly from global AsyncStorage keys such as `@quietgrove:userPlants`, `@quietgrove:userName`, `@quietgrove:points`, and related profile keys on every app load regardless of which human is currently using the device (`artifacts/mobile/context/GardenContext.tsx:404-495`). Journal entries are stored separately under a single global key, `@quietgrove:journalEntries`, and are likewise loaded without any account scoping (`artifacts/mobile/hooks/useJournal.ts:12-29`).

After sign-out, the next device user does not need to authenticate to access that retained data. The sign-in screen's “Sign up later” button simply sets `isGuest` and routes into the tab UI (`artifacts/mobile/app/(auth)/sign-in.tsx:361-364`). The tab layout explicitly allows access whenever either `isSignedIn` or `isGuest` is true (`artifacts/mobile/app/(tabs)/_layout.tsx:334-377`). Once inside, multiple screens render the retained state directly, including the profile screen's stats and plant counts (`artifacts/mobile/app/(tabs)/profile.tsx:72-103,276-281`) and the journal screen, which loads and displays all stored entries (`artifacts/mobile/app/(tabs)/journal.tsx:152-156` and `44-77`).

A realistic exploit on a shared family tablet is straightforward: Alice signs out of MossLight, hands the device to Bob, and Bob taps “Sign up later.” Bob is then dropped into the prior local session with Alice's plant collection, nicknames, notes, care history-derived stats, and journal entries still present. This is a cross-user confidentiality failure in the app's stated shared-device threat model, not merely local tampering, because ordinary app navigation exposes one person's cached content to the next person using the same installation.
  Files: artifacts/mobile/app/(tabs)/profile.tsx, artifacts/mobile/app/(auth)/sign-in.tsx, artifacts/mobile/app/(tabs)/_layout.tsx, artifacts/mobile/context/GardenContext.tsx, artifacts/mobile/hooks/useJournal.ts, artifacts/mobile/app/(tabs)/journal.tsx

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #121 — Sync the last-viewed premium skin position across devices too
- **Status:** CANCELLED
- **Created:** 2026-05-29T03:05:51.035Z
- **Updated:** 2026-05-29T03:53:45.478Z
- **Depends on:** #103
- **Proposed from:** #103
- **Category:** next_steps

# Sync the last-viewed premium skin position across devices

  ## What & Why
  The `@quietgrove:last_preview_skin_index` key is already in `ALL_PROFILE_KEYS` and syncs via cloud, but the in-memory React state (`lastPreviewSkinIndex`) is only hydrated from AsyncStorage on mount. If a cloud restore happens mid-session (e.g. conflict resolution), the state is not refreshed. This is the premium-skin counterpart to task-103's free-skin fix.

  ## Done looks like
  - After a cloud restore, the profile screen re-reads `last_preview_skin_index` from AsyncStorage and updates the in-memory state, so the carousel opens at the correct skin on all devices.

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — `LAST_PREVIEW_SKIN_INDEX_KEY`, `lastPreviewSkinIndex` state, useEffect at line 265
  - `artifacts/mobile/context/GardenContext.tsx` — `restoreFromCloudSnapshot`

---

#### #129 — Apply velocity-scaled spring to the snap-back when a swipe is cancelled
- **Status:** CANCELLED
- **Created:** 2026-05-29T04:20:06.498Z
- **Updated:** 2026-05-29T05:21:58.570Z
- **Depends on:** #107
- **Proposed from:** #107
- **Category:** incomplete_scope

# Apply velocity-scaled spring to the snap-back when a swipe is cancelled

  ## What & Why
  The `onPanResponderRelease` and `onPanResponderTerminate` handlers that snap the panel back to its original position (when a swipe doesn't cross the threshold) still use fixed spring values (stiffness: 240, damping: 22). A fast-flick that doesn't travel far enough to trigger navigation should still snap back crisply, consistent with the velocity-scaled spring now used in `jumpToPreviewSkin`.

  ## Done looks like
  - The snap-back spring in `onPanResponderRelease` (the else branch) and `onPanResponderTerminate` scale stiffness/damping based on `gs.vx`
  - Fast cancelled flicks feel tight; slow swipes settle gently

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — `previewPanResponder` definition, the two `Animated.spring` calls in the release/terminate handlers

---

#### #130 — Add dot-scrub support to the free skin carousel so you can drag across dots to preview skins quickly
- **Status:** CANCELLED
- **Created:** 2026-05-29T04:37:00.620Z
- **Updated:** 2026-05-29T05:24:48.087Z
- **Depends on:** #108
- **Proposed from:** #108
- **Category:** next_steps

# Add dot-scrub support to the free skin carousel

  ## What & Why
  The premium skin carousel supports a press-and-hold scrub gesture on the dot indicator row, letting users quickly drag across all skins with haptic feedback. The new free skin carousel has tappable dots but not the scrub gesture, making the experience feel uneven.

  ## Done looks like
  - Press-and-hold on the free skin dots row activates scrub mode
  - Dragging across dots navigates skins with haptic feedback (same as premium carousel)
  - Releasing cancels scrub mode

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — see `dotBarTouchHandlers` and `isScrubbing` patterns (lines ~167–210) for the premium carousel scrub implementation; replicate for the free skin dot row

---

#### #131 — Show a tooltip with the skin name while scrubbing across the free skin carousel dots
- **Status:** CANCELLED
- **Created:** 2026-05-29T04:37:00.620Z
- **Updated:** 2026-05-29T05:24:48.087Z
- **Depends on:** #108
- **Proposed from:** #108
- **Category:** next_steps

# Show a tooltip with the skin name while scrubbing across the free skin carousel dots

  ## What & Why
  Once scrub support is added to the free skin carousel's dot row, users scrubbing quickly won't be able to read the skin name in time. A tooltip above the dot bar showing the current skin name (matching the planned `scrub handle tooltip` task for the premium carousel) gives context while scrubbing.

  ## Done looks like
  - A small label/tooltip appears above the dot row during scrub, showing the skin name at the current dot position
  - It disappears once the user lifts their finger

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — free skin carousel section (around line 845); see existing tooltip task for premium carousel for design reference

---

#### #141 — Scroll the screen to the skin carousel before the highlight plays
- **Status:** CANCELLED
- **Created:** 2026-05-29T14:11:19.530Z
- **Updated:** 2026-05-29T14:26:24.046Z
- **Depends on:** #120
- **Proposed from:** #120
- **Category:** next_steps

# Scroll the screen to the skin carousel before the highlight plays

  ## What & Why
  On a cold restart the profile screen opens at the top. The highlight animation fires on the free-skin carousel (further down the page), but the user is looking at the top of the screen and never sees it. The fix should scroll the main ScrollView to the carousel section before (or together with) the highlight pulse.

  ## Done looks like
  - After AsyncStorage hydration triggers the highlight, the main ScrollView scrolls to the carousel row so the animated highlight is always visible
  - The scroll happens before the highlight opacity animation starts (matching the existing delay pattern in triggerSkinHighlight)

  ## Relevant files
  - `artifacts/mobile/app/(tabs)/profile.tsx` — triggerSkinHighlight (~line 307), scrollViewRef (~line 147), freeSkinHydrated effect (~line 323)

---

#### #159 — Release Signing Integrity
- **Status:** CANCELLED
- **Created:** 2026-05-30T12:57:05.393Z
- **Updated:** 2026-05-30T13:01:26.583Z

Vulnerabilities in the iOS release pipeline that expose signing material and weaken the integrity of production mobile builds.

Vulnerabilities to fix:

1. [High] Workspace-stored iOS signing credentials allow unauthorized production build signing
  Anyone with access to the Replit workspace can take the app's real iPhone distribution certificate, its provisioning profile, and the password needed to unlock them, then use them to sign their own MossLight iOS builds as if they came from the legitimate publisher. This breaks release integrity because an unauthorized party can produce production-signed builds for the real app identifier.

The production EAS profile is explicitly configured to use local signing material (`artifacts/mobile/eas.json:28-37`), and the workflow invokes that profile for production iOS builds (`.replit:34-47`). The signing assets are present in an accessible workspace path: `artifacts/mobile/dist_cert.p12`, `artifacts/mobile/dist_profile.mobileprovision`, and `artifacts/mobile/credentials.json`. The password protecting the PKCS#12 bundle is recoverable in two places: `artifacts/mobile/credentials.json:2-7` and the hardcoded `P12_PASSWORD = "MossLightBuild2025"` in `scripts/setup-ios-credentials.mjs:15`. The same script intentionally copies freshly minted Apple signing artifacts into the workspace (`scripts/setup-ios-credentials.mjs:178-193`) and then skips regeneration whenever those files already exist (`scripts/setup-ios-credentials.mjs:28-35`), so the sensitive material persists instead of remaining ephemeral.

The exposed files are not dummy placeholders. The committed workspace currently contains a valid App Store distribution certificate and matching provisioning profile. Inspection of `artifacts/mobile/dist_cert.p12` with the published password shows a certificate for `iPhone Distribution: Michaela Walcott (QHS9VF3VX5)` issued by Apple, valid from 2026-05-30 to 2027-05-30. Inspection of `artifacts/mobile/dist_profile.mobileprovision` shows an App Store profile for `QHS9VF3VX5.com.quietgrovebotania.mosslightapp` with `aps-environment` set to `production`, team identifier `QHS9VF3VX5`, and the same 2027-05-30 expiry. In other words, a workspace reader can recover the exact private signing identity used for the production app.

Exploitation is straightforward: a collaborator, compromised agent session, or anyone else who can read workspace files can copy `dist_cert.p12`, `dist_profile.mobileprovision`, and the password from `credentials.json` or `setup-ios-credentials.mjs`, then use standard Apple tooling or EAS/local signing to build and sign arbitrary code for `com.quietgrovebotania.mosslightapp`. Even without separate App Store Connect submission credentials, this is still a real production-integrity issue because it enables unauthorized creation of publisher-signed artifacts, undermines trust in the release pipeline, and materially lowers the bar for any future compromise of TestFlight/App Store submission steps. The fix is to remove the signing bundle and provisioning profile from the workspace, rotate the exposed certificate/profile, and keep signing material only in dedicated secrets/CI storage rather than under `artifacts/mobile/`.
  Files: .replit, artifacts/mobile/eas.json, artifacts/mobile/credentials.json, artifacts/mobile/dist_cert.p12, artifacts/mobile/dist_profile.mobileprovision, scripts/setup-ios-credentials.mjs

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #163 — Release Signing Integrity
- **Status:** CANCELLED
- **Created:** 2026-05-30T13:01:15.521Z
- **Updated:** 2026-05-30T13:01:30.578Z

Vulnerabilities in the iOS release pipeline that expose signing material and weaken the integrity of production mobile builds.

Vulnerabilities to fix:

1. [High] Workspace-stored iOS signing credentials allow unauthorized production build signing
  Anyone with access to the Replit workspace can take the app's real iPhone distribution certificate, its provisioning profile, and the password needed to unlock them, then use them to sign their own MossLight iOS builds as if they came from the legitimate publisher. This breaks release integrity because an unauthorized party can produce production-signed builds for the real app identifier.

The production EAS profile is explicitly configured to use local signing material (`artifacts/mobile/eas.json:28-37`), and the workflow invokes that profile for production iOS builds (`.replit:34-47`). The signing assets are present in an accessible workspace path: `artifacts/mobile/dist_cert.p12`, `artifacts/mobile/dist_profile.mobileprovision`, and `artifacts/mobile/credentials.json`. The password protecting the PKCS#12 bundle is recoverable in two places: `artifacts/mobile/credentials.json:2-7` and the hardcoded `P12_PASSWORD = "MossLightBuild2025"` in `scripts/setup-ios-credentials.mjs:15`. The same script intentionally copies freshly minted Apple signing artifacts into the workspace (`scripts/setup-ios-credentials.mjs:178-193`) and then skips regeneration whenever those files already exist (`scripts/setup-ios-credentials.mjs:28-35`), so the sensitive material persists instead of remaining ephemeral.

The exposed files are not dummy placeholders. The committed workspace currently contains a valid App Store distribution certificate and matching provisioning profile. Inspection of `artifacts/mobile/dist_cert.p12` with the published password shows a certificate for `iPhone Distribution: Michaela Walcott (QHS9VF3VX5)` issued by Apple, valid from 2026-05-30 to 2027-05-30. Inspection of `artifacts/mobile/dist_profile.mobileprovision` shows an App Store profile for `QHS9VF3VX5.com.quietgrovebotania.mosslightapp` with `aps-environment` set to `production`, team identifier `QHS9VF3VX5`, and the same 2027-05-30 expiry. In other words, a workspace reader can recover the exact private signing identity used for the production app.

Exploitation is straightforward: a collaborator, compromised agent session, or anyone else who can read workspace files can copy `dist_cert.p12`, `dist_profile.mobileprovision`, and the password from `credentials.json` or `setup-ios-credentials.mjs`, then use standard Apple tooling or EAS/local signing to build and sign arbitrary code for `com.quietgrovebotania.mosslightapp`. Even without separate App Store Connect submission credentials, this is still a real production-integrity issue because it enables unauthorized creation of publisher-signed artifacts, undermines trust in the release pipeline, and materially lowers the bar for any future compromise of TestFlight/App Store submission steps. The fix is to remove the signing bundle and provisioning profile from the workspace, rotate the exposed certificate/profile, and keep signing material only in dedicated secrets/CI storage rather than under `artifacts/mobile/`.
  Files: .replit, artifacts/mobile/eas.json, artifacts/mobile/credentials.json, artifacts/mobile/dist_cert.p12, artifacts/mobile/dist_profile.mobileprovision, scripts/setup-ios-credentials.mjs

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #176 — Release Pipeline Credentials
- **Status:** CANCELLED
- **Created:** 2026-06-04T13:21:39.033Z
- **Updated:** 2026-06-04T13:38:04.766Z

Vulnerabilities that expose production mobile signing or submission material and can undermine release integrity.

Vulnerabilities to fix:

1. [High] Production iOS signing key and provisioning profile left in workspace
  Production iOS release credentials were materialized into the project workspace and left there after use. Anyone who can read the workspace can recover the MossLight App Store signing private key and provisioning profile, which undermines the integrity of future iOS releases.

The release pipeline intentionally writes live signing material into reusable project paths under `artifacts/mobile`: `scripts/setup-ios-credentials.mjs` creates `artifacts/mobile/.eas-creds/dist_cert.p12`, `artifacts/mobile/.eas-creds/dist_profile.mobileprovision`, and `artifacts/mobile/credentials.json` containing the p12 password (`scripts/setup-ios-credentials.mjs:43-46`, `295-316`). Although `.replit` attempts to delete these files after `eas build` finishes (`.replit:31-33`), the credentials are currently still present in the workspace, which proves the cleanup is best-effort rather than guaranteed.

This is a concrete exposure, not a hypothetical one. `artifacts/mobile/credentials.json` contains the decryption password for `.eas-creds/dist_cert.p12`, and that bundle decrypts to a private key and certificate for `iPhone Distribution: Michaela Walcott (QHS9VF3VX5)` valid until `2027-06-04`. The accompanying provisioning profile is an App Store profile for `QHS9VF3VX5.com.quietgrovebotania.mosslightapp`, also valid until `2027-06-04`, with team `QHS9VF3VX5` and app name `MossLight App`. In other words, a workspace reader can extract the real production signing identity for the shipped iOS app.

An attacker with repository/workspace access could reuse these artifacts to sign a malicious MossLight iOS build for the production bundle identifier, replace expected build outputs, or prepare a trojanized release for submission once they obtain any App Store Connect submission path. Even without the App Store Connect API private key, exposure of the signing private key and matching provisioning profile is a high-impact release-integrity failure because it gives attackers the same cryptographic identity used by the legitimate production iOS release process. The root cause is storing live signing material in reusable workspace paths instead of keeping it exclusively in ephemeral secrets-backed storage with guaranteed cleanup.
  Files: .replit, scripts/setup-ios-credentials.mjs, artifacts/mobile/credentials.json, artifacts/mobile/.eas-creds/dist_cert.p12, artifacts/mobile/.eas-creds/dist_profile.mobileprovision

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #177 — Fix 5 dependency vulnerabilities
- **Status:** CANCELLED
- **Created:** 2026-06-04T13:21:41.920Z
- **Updated:** 2026-06-04T13:38:08.245Z

Fix the following dependency vulnerabilities:

- [High] @xmldom/xmldom@0.7.13 (GHSA-2v35-w6hq-6mfw@@xmldom/xmldom-0.7.13)
- [High] @xmldom/xmldom@0.7.13 (GHSA-f6ww-3ggp-fr8h@@xmldom/xmldom-0.7.13)
- [High] @xmldom/xmldom@0.7.13 (GHSA-j759-j44w-7fr8@@xmldom/xmldom-0.7.13)
- [High] @xmldom/xmldom@0.7.13 (GHSA-wh4c-j3r5-mjhp@@xmldom/xmldom-0.7.13)
- [High] @xmldom/xmldom@0.7.13 (GHSA-x6wf-f3px-wcqx@@xmldom/xmldom-0.7.13)

---

#### #205 — Write the Google Play Store listing copy from the project history
- **Status:** CANCELLED
- **Created:** 2026-07-03T14:03:58.852Z
- **Updated:** 2026-07-03T14:12:21.792Z
- **Depends on:** #204
- **Proposed from:** #204
- **Category:** next_steps

# Google Play Store Listing Copy

  ## What & Why
  The user is setting up a Google Play Developer account and needs a store listing before they can publish. The full changelog now documents every shipped feature clearly, making it the perfect source for the short description, long description, and feature highlights needed for Google Play.

  ## Done looks like
  - A short description (80 chars max) ready to paste into Google Play Console
  - A long description (up to 4000 chars) covering the app's purpose, key features, and premium offerings
  - A "What's New" release notes entry for the first Android submission
  - All copy written in plain, approachable language matching the MossLight brand voice (calm, nature-focused, personal)

  ## Out of scope
  - Screenshots or graphic assets
  - Keyword research / ASO optimization

  ## Relevant files
  - `CHANGELOG.txt` (full feature history to draw from)
  - `artifacts/mobile/app.config.ts` (current version, bundle ID, app name)

---

#### #212 — Fix duplicate plant photo records blocking reliable app checks
- **Status:** CANCELLED
- **Created:** 2026-08-23T22:15:03.634Z
- **Updated:** 2026-08-23T22:20:36.744Z
- **Category:** tech_debt

# Fix duplicate plant photo records blocking reliable app checks

## What & Why
The mobile app's TypeScript check currently fails before it can give a clean reliability signal because three image-map keys are duplicated. Removing or reconciling the repeated catalog photo records restores a clean validation baseline without changing the user-selected photo persistence behavior.

## Done looks like
- The repeated image-map keys are reconciled intentionally
- Mobile TypeScript validation completes without duplicate-key errors
- Each affected plant still resolves to its intended catalog image

## Relevant files
- artifacts/mobile/data/plant-photos.ts

---

#### #215 — Fix duplicate plant photo keys blocking type checks
- **Status:** CANCELLED
- **Created:** 2026-08-23T22:36:22.241Z
- **Updated:** 2026-08-23T22:59:43.059Z
- **Depends on:** #209
- **Proposed from:** #209
- **Category:** tech_debt

# Fix duplicate plant photo keys blocking type checks

## What & Why
The mobile TypeScript check is currently blocked by duplicate object keys for three plant photo entries. Removing the duplicates will restore a clean validation signal for future mobile changes.

## Done looks like
- The duplicate aeonium photo keys are resolved without changing the intended image mapping.
- pnpm --filter @workspace/mobile run typecheck completes successfully.

## Relevant files
- artifacts/mobile/data/plant-photos.ts

---

#### #218 — Confirm the release build’s typography on a real Android phone
- **Status:** CANCELLED
- **Created:** 2026-08-23T23:21:36.690Z
- **Updated:** 2026-08-24T00:00:46.876Z
- **Depends on:** #214
- **Proposed from:** #214
- **Category:** incomplete_scope

# Confirm the release build’s typography on a real Android phone

## What & Why
This workspace has no Android SDK, ADB, emulator, or connected phone, so the native Android portion of the typography release check could not be completed here. The Android-targeted Metro bundle succeeds and the 400×720 browser surrogate now passes, but real Android font metrics must still be checked before release.

## Done looks like
- Install the release candidate on a physical Android phone or Android emulator
- Check Home, Garden, Discover, Journal, Profile, Add Plant, Import Plant, Plant Detail, Sign In, Welcome, Feature Tour, and Paywall
- Confirm GreatVibes and CormorantGaramond italic glyphs have no cropping, clipping, overlap, or unusual spacing
- Record the device model, Android version, screen/font scaling setting, and any findings

## Relevant files
- artifacts/mobile/components/WelcomeScreen.tsx
- artifacts/mobile/components/FeatureTour.tsx
- artifacts/mobile/components/PaywallModal.tsx
- artifacts/mobile/app/(auth)/sign-in.tsx
- artifacts/mobile/app/(tabs)/profile.tsx
- artifacts/mobile/app/add-plant.tsx

---

#### #219 — Keep plant-photo catalog duplicates from blocking release checks
- **Status:** CANCELLED
- **Created:** 2026-08-23T23:21:36.690Z
- **Updated:** 2026-08-24T00:00:46.876Z
- **Depends on:** #214
- **Proposed from:** #214
- **Category:** tech_debt

# Keep plant-photo catalog duplicates from blocking release checks

## What & Why
The mobile type check currently stops before validating the app because three duplicate plant-photo entries are present. Removing or safely consolidating those duplicate mappings will restore a reliable release check and make future UI validation failures easier to catch.

## Done looks like
- The duplicate photo mappings are reconciled without removing intended plant images
- pnpm --filter @workspace/mobile run typecheck completes successfully
- Plant image lookups for the affected Aeonium varieties still resolve correctly

## Relevant files
- artifacts/mobile/data/plant-photos.ts

---

#### #221 — Keep unavailable Apple sign-in from leading users to a failed flow
- **Status:** CANCELLED
- **Created:** 2026-08-24T00:25:19.390Z
- **Updated:** 2026-08-24T00:25:25.200Z
- **Category:** tech_debt

# Keep unavailable Apple sign-in from leading users to a failed flow

## What & why
The sign-in screen currently renders Apple sign-in while the source explicitly says Apple SSO is not enabled in Clerk. A user can choose Apple and receive a provider failure at the first screen of the app.

## Done looks like
- Apple sign-in is hidden or clearly disabled until the Clerk Apple provider is configured, OR the provider is configured and confirmed through a complete native sign-in flow.
- The sign-in screen never offers an authentication option known to fail.
- Sign-in failures use short, user-friendly recovery guidance instead of raw provider messages where practical.

## Relevant files
- artifacts/mobile/app/(auth)/sign-in.tsx
- artifacts/mobile/app/_layout.tsx
- MossLight_User_Impact_Audit.txt

---

#### #222 — Make slow startup recover instead of leaving users at a blank or endless loading screen
- **Status:** CANCELLED
- **Created:** 2026-08-24T00:25:19.390Z
- **Updated:** 2026-08-24T00:25:25.200Z
- **Category:** tech_debt

# Make slow startup recover instead of leaving users at a blank or endless loading screen

## What & why
The root layout returns no UI while fonts or the Clerk key are unresolved. The branded loading screen has a Try again action, but it only restarts its visual timers and does not retry the underlying readiness work. The browser surrogate also intermittently showed a blank screen during Metro/API instability, which makes this recovery gap especially visible.

## Done looks like
- Root startup always shows a branded loading or recovery state rather than a blank screen.
- Retry performs a meaningful recovery action for the relevant startup dependency, or offers a dependable offline/restart route.
- A slow or failed initial dependency gives the user clear next steps.
- Browser-preview work remains coordinated with the existing API-preview task rather than duplicating it.

## Relevant files
- artifacts/mobile/app/_layout.tsx
- artifacts/mobile/app/(auth)/_layout.tsx
- artifacts/mobile/components/BrandedLoadingScreen.tsx
- MossLight_User_Impact_Audit.txt

---

#### #224 — Confirm the next TestFlight release keeps its existing Apple signing identity
- **Status:** CANCELLED
- **Created:** 2026-08-24T02:18:32.123Z
- **Updated:** 2026-08-24T03:21:36.217Z
- **Depends on:** #223
- **Proposed from:** #223
- **Category:** test_gaps

# Confirm the next TestFlight release keeps its existing Apple signing identity

## What & Why
The release workflow now uses frozen EAS-managed credentials, so it cannot silently create or replace Apple signing assets. The next intentional TestFlight build should confirm the stored certificate and provisioning profile are accepted unchanged by EAS and Apple.

## Done looks like
- A production iOS EAS build completes using the existing EAS-managed certificate and provisioning profile
- The EAS and Apple build logs show no certificate or provisioning-profile creation, replacement, or revocation
- No local signing bundle or credentials file remains after the workflow exits

## Relevant files
- .replit
- artifacts/mobile/eas.json
- scripts/setup-ios-credentials.mjs

---

#### #225 — Restore the Apple signing identity needed for TestFlight releases
- **Status:** CANCELLED
- **Created:** 2026-08-24T03:00:10.366Z
- **Updated:** 2026-08-24T03:21:36.217Z
- **Depends on:** #224
- **Proposed from:** #224
- **Category:** incomplete_scope

# Restore the Apple signing identity needed for TestFlight releases

## What & Why
The frozen production iOS workflow now authenticates to Apple and explicitly refuses to create signing assets. EAS reports that this project has no stored remote build credentials and its guided flow offers to generate a new distribution certificate. Restoring the existing certificate/private key and matching App Store provisioning profile into EAS is required before a TestFlight build can proceed without changing the Apple signing identity.

## Done looks like
- The existing Apple Distribution certificate, its private key, and the existing App Store provisioning profile are deliberately attached to the EAS project
- No certificate or provisioning profile is generated, replaced, or revoked
- The frozen production build completes and EAS reports it is using remote credentials

## Relevant files
- .replit
- artifacts/mobile/eas.json
- scripts/setup-ios-credentials.mjs

---

#### #226 — Keep garden changes safe when users use more than one device
- **Status:** CANCELLED
- **Created:** 2026-08-25T04:23:37.011Z
- **Updated:** 2026-08-25T04:24:21.376Z
- **Category:** tech_debt

# Keep garden changes safe when users use more than one device

## What & Why
MossLight currently syncs one whole profile snapshot at a time. If two devices change the garden near the same time, the newer upload can silently overwrite details from the other device. Adding a clear revision/conflict protocol protects a user's garden history as cross-device use grows.

## Done looks like
- Cloud profile writes include a revision or equivalent concurrency check
- The app can identify a stale write instead of silently replacing newer cloud data
- Conflict handling preserves or clearly reconciles plant and journal changes
- Automated coverage demonstrates the expected two-device behavior

## Relevant files
- artifacts/api-server/src/routes/profile.ts
- lib/db/src/schema/profiles.ts
- artifacts/mobile/utils/api.ts
- artifacts/mobile/context/GardenContext.tsx
- artifacts/mobile/components/MergePreviewSheet.tsx

---

#### #227 — Protect private files before MossLight adds cloud photo backup
- **Status:** CANCELLED
- **Created:** 2026-08-25T04:23:37.011Z
- **Updated:** 2026-08-25T04:24:21.376Z
- **Category:** tech_debt

# Protect private files before MossLight adds cloud photo backup

## What & Why
The object-storage routes include a signed upload URL and a route labeled for private objects, but the reviewed route declarations do not currently require an authenticated user or verify ownership. This needs a clear access policy before those routes support user photos or other personal files.

## Done looks like
- Upload URL issuance requires a signed-in user
- File size and content-type limits are enforced server-side
- Private object reads verify the requesting user's ownership
- Object paths, expiry, and cleanup rules are documented and tested

## Relevant files
- artifacts/api-server/src/routes/storage.ts
- artifacts/api-server/src/lib/objectStorage.ts
- artifacts/api-server/src/lib/auth.ts

---

#### #228 — Let users keep their journal photos when they switch devices
- **Status:** CANCELLED
- **Created:** 2026-08-25T04:23:37.011Z
- **Updated:** 2026-08-25T04:24:21.376Z
- **Category:** next_steps

# Let users keep their journal photos when they switch devices

## What & Why
Journal and plant photos are safely copied into app-owned local storage, but only the local URI string can be included in a profile snapshot. A person restoring their garden on another device receives the journal entry text without the original photo bytes.

## Done looks like
- Authenticated photo upload/download uses the secured object-storage policy
- Journal and plant records retain a stable remote photo reference
- Photo backup and restore work across signed-in devices
- The privacy policy clearly explains the new cloud-media behavior

## Relevant files
- artifacts/mobile/utils/persistPhoto.ts
- artifacts/mobile/hooks/useJournal.ts
- artifacts/mobile/app/plant/[id].tsx
- artifacts/mobile/app/(tabs)/journal.tsx
- artifacts/api-server/src/routes/storage.ts

---

#### #231 — Make the MossLight field guide ready for a printer and storefront
- **Status:** CANCELLED
- **Created:** 2026-08-25T06:54:30.046Z
- **Updated:** 2026-08-25T06:54:38.533Z
- **Category:** next_steps

# Make the MossLight field guide ready for a printer and storefront

## What & Why
The manuscript now has validated print and Kindle editions, but a physical release still needs a production cover, spine, back-cover copy, ISBN/barcode placeholder, and a print-proof pass against the selected 8 × 10 trim. This turns the manuscript into a package that can be uploaded to a printer or book storefront.

## Done looks like
- A full-wrap 8 × 10 cover file with calculated spine options
- Back-cover description and author bio copy
- A print-proof checklist and revised interior PDF if issues are found
- Kindle metadata package with keywords, description, and categories

## Relevant files
- exports/mosslight-field-guide-cultivating-with-curiosity-print.pdf
- exports/mosslight-field-guide-cultivating-with-curiosity-print.html
- exports/mosslight-field-guide-cultivating-with-curiosity-kindle.epub

---

#### #232 — Keep MossLight content factual and release-ready
- **Status:** CANCELLED
- **Created:** 2026-08-25T07:08:51.086Z
- **Updated:** 2026-08-25T07:09:52.231Z
- **Category:** incomplete_scope

# Verify MossLight’s Scientific Content

## What & Why
Review the complete MossLight learning and plant-care experience before its next iOS submission, then establish one authoritative, public source hub for citations. The mobile app and browser Learn experience should remain calm and readable: they will point to the same source hub and show brief source context only where it meaningfully changes a care or safety decision. The full bibliography, claim notes, revision history, and methodology should live once on the website.

## Done looks like
- Every learning, plant-care, pest, soil, and field-journal claim has been inventoried and assigned either a verified source, a clearly labeled observation/practice status, or a rewrite/removal decision
- Safety, toxicity, invasive-species, food-safety, and taxonomy-sensitive content uses current authoritative sources and cautious language
- The browser has a single accessible Sources & Methodology hub with searchable references, a revision date, and a clear explanation of the notation used across MossLight
- Native and browser Learn surfaces link to the same source hub without repeating long citations in every card or article
- The shared content has no duplicate source records and uses one stable source identifier per reference
- Native and browser interfaces are reviewed for readable source access, larger-text support, correct external-link behavior, and uncluttered presentation
- An end-to-end release review covers core garden flows, plant catalog, Learn/source access, journal, profile, purchase/guest boundaries, and browser-local boundaries
- The iOS build is reviewed for release-blocking errors and is ready for a new TestFlight submission after content verification is complete

## Out of scope
- Claiming that any general-audience plant guide can replace species-specific professional, veterinary, legal, food-safety, or local regulatory advice
- Duplicating a full bibliography inside every app entry
- New commerce, account, or cloud-sync features unrelated to the editorial/release review
- Rebuilding the browser-parity work that is already being merged

## Steps
1. **Content evidence inventory** — Create a structured claim and source inventory for the shared learning catalogue, plant records, soil guides, pests, and field notes; classify each item as botanical fact, adaptable care practice, or safety-sensitive guidance.
2. **Scientific and safety audit** — Research and verify high-consequence, quantitative, taxonomic, and time-sensitive claims against botanic-garden, extension, peer-reviewed, veterinary, food-safety, conservation, and regulatory sources; rewrite or remove unsupported statements.
3. **Canonical source hub** — Build one browser-accessible Sources & Methodology surface with source IDs, reference details, revision date, topic filters, and an editorial-method explanation; keep it as the single public bibliography.
4. **Uncluttered app linking** — Add concise source access in native and browser Learn experiences that points to the canonical hub, using clear labels and accessibility-safe links instead of repeated inline bibliographies.
5. **Cross-artifact quality review** — Check native and browser content parity, link behavior, typography/accessibility, catalog facts, safety copy, local-only disclosures, and app-store-sensitive metadata or claims.
6. **Release gate and resubmission readiness** — Run the relevant automated checks and manual smoke paths, document unresolved items, then prepare the verified iOS build for TestFlight submission using the existing release workflow.

## Relevant files
- `artifacts/mobile/app.config.ts`
- `artifacts/mobile/app/(tabs)/discover.tsx`
- `artifacts/mobile/data/tips.ts`
- `artifacts/mobile/data/plants.ts`
- `artifacts/mobile/data/soil-guide.ts`
- `artifacts/mobile/data/pests.ts`
- `artifacts/mobile/data/field-journal.ts`
- `artifacts/mosslight-preview/src/pages/learn.tsx`
- `artifacts/mosslight-preview/src/pages/plant-detail.tsx`
- `artifacts/mosslight-preview/src/pages/profile.tsx`
- `docs/mosslight-field-guide-fact-check-notes.md`
- `scripts/bump-build-number.mjs`

---

#### #233 — Bring the Kindle field guide up to the illustrated print edition
- **Status:** CANCELLED
- **Created:** 2026-08-25T07:08:51.086Z
- **Updated:** 2026-08-25T07:09:50.576Z
- **Category:** next_steps

# Bring the Kindle field guide up to the illustrated print edition

## What & Why
The physical edition now has full-color MossLight art, app plant portraits, advanced reference sections, notation, and expanded source notes. The existing Kindle file is still the earlier text-first edition. A matching reflowable edition will give readers the same authority and beauty on e-readers without forcing a print layout onto a small screen.

## Done looks like
- Kindle navigation includes the advanced field notes and source record
- Illustrations have captions, meaningful alt text, and e-reader-friendly placement
- The same corrections and citation notation appear in the ebook
- EPUB metadata and structure validate successfully

## Out of scope
- Retailer upload or account setup

## Steps
1. Rebuild the ebook structure around the revised physical edition content
2. Adapt illustrations and captions for reflowable reading and accessibility
3. Carry across annotations, sources, and corrected language
4. Validate the final EPUB package and metadata

## Relevant files
- `exports/mosslight-field-guide-cultivating-with-curiosity-kindle.epub`
- `exports/mosslight-field-guide-cultivating-with-curiosity-illustrated-print.html`
- `docs/mosslight-field-guide-fact-check-notes.md`

---

#### #235 — Submit the verified Learn update to TestFlight
- **Status:** CANCELLED
- **Created:** 2026-08-25T07:26:39.970Z
- **Updated:** 2026-08-25T07:44:38.690Z
- **Depends on:** #234
- **Proposed from:** #234
- **Category:** next_steps

# Submit the verified Learn update to TestFlight

## What & Why
Once the physical-device or simulator review is complete, publish the native iOS update so the corrected Learn organization, science-backed guidance, source panel, and accessibility labels reach testers.

## Done looks like
- Confirm the final device review has no blocking Learn, navigation, accessibility, or source-link issues
- Start the project’s Expo Launch iOS publishing flow
- Verify the submitted build uses the updated native Learn content and correct release metadata
- Confirm the TestFlight build is available for internal testing

## Out of scope
- Browser MossLight changes
- Kindle or physical-book export changes

## Relevant files
- `artifacts/mobile/app.config.ts`
- `artifacts/mobile/app/(tabs)/discover.tsx`
- `artifacts/mobile/components/SourcesMethodologyCard.tsx`
- `artifacts/mobile/data/tips.ts`
- `artifacts/mobile/data/sources.ts`

---

#### #237 — Catch release workflow ordering regressions before TestFlight uploads
- **Status:** CANCELLED
- **Created:** 2026-08-25T07:47:43.377Z
- **Updated:** 2026-08-25T08:11:32.364Z
- **Depends on:** #236
- **Proposed from:** #236
- **Category:** test_gaps

# Catch release workflow ordering regressions before TestFlight uploads

## What & Why
The release flow now depends on build completion before submission, but a future workflow edit could accidentally restore parallel execution or remove the build-number report. A lightweight validation check would catch that before an Apple upload is attempted.

## Done looks like
- A repeatable check inspects .replit without starting EAS.
- It fails if the combined Project workflow is parallel or if submission is not after the iOS build.
- It fails if the submit command no longer reports the build number before invoking eas submit.

## Relevant files
- .replit
- scripts/bump-build-number.mjs

---

#### #238 — Give every companion a true species-specific animation
- **Status:** CANCELLED
- **Created:** 2026-08-25T08:15:41.876Z
- **Updated:** 2026-08-25T08:53:05.040Z

# Animate every companion\n\n## What & Why\nBring every MossLight companion up to the fox's authored animation quality. The live cottage, companion previews, and skin previews should use recognizable species-specific motion instead of relying on a single generic float or a mostly static procedural drawing.\n\n## Done looks like\n- Every available FriendType has a deliberate, visibly different motion identity in the live cottage, while preserving the existing traversal paths, visibility rules, skin resolution, and unlock behavior.\n- Mushroom variants have layered growth, settling, idle, and retreat motion; the nighttime glowing treatment pulses as part of the same animation rather than reading as a static halo.\n- The chrysalis has organic hanging motion with small irregular life cues, not only a uniform sway and scale pulse.\n- Companion and skin preview cards use the same species-aware motion language as the cottage instead of one shared float animation.\n- Motion stays smooth on supported devices, uses native-driver-safe primitives where possible, cleans up on unmount, and does not create excessive simultaneous animation work.\n- Existing skins, premium gating, appearance cycling, and companion preferences continue to work unchanged.\n\n## Out of scope\n- Adding new companion species, unlock tiers, purchases, or account fields.\n- Replacing the existing companion art catalog or introducing a separate animation file format.\n- Changing the timing/frequency rules that control how often companions appear unless a small adjustment is required for the new motion to read correctly.\n\n## Steps\n1. **Inventory the motion surfaces** -- Audit every live companion, mushroom state, chrysalis state, and preview/card rendering path so no companion is left on the generic fallback animation.\n2. **Build species-aware motion** -- Extend the shared motion system with distinct locomotion and idle cues for each companion, and add layered procedural motion for mushrooms and the chrysalis while keeping the current state and visibility contracts intact.\n3. **Unify previews with live motion** -- Update companion and skin preview rendering to select the same species-aware animation profile, with a lightweight bounded version suitable for small cards.\n4. **Verify behavior and performance** -- Type-check the mobile package, test all companion/skin states and light/dark mushroom behavior, verify cleanup and reduced-motion-safe behavior if the app already exposes that preference, and inspect the resulting animations in the relevant preview surfaces.\n\n## Relevant files\n- `artifacts/mobile/components/CottageCritters.tsx:19-777,788-1098`\n- `artifacts/mobile/components/FriendIllustration.tsx:1-124`\n- `artifacts/mobile/lib/critterSkins.ts:1-130`\n- `artifacts/mobile/context/GardenContext.tsx:64-213`\n- `artifacts/mobile/components/PaywallModal.tsx:350-420,540-591`

---

#### #239 — SEO scan
- **Status:** CANCELLED
- **Created:** 2026-08-25T21:55:43.126Z
- **Updated:** 2026-08-26T21:27:18.056Z

Run an in-depth scan across the entire project

---

#### #240 — Fix 15 dependency vulnerabilities
- **Status:** CANCELLED
- **Created:** 2026-08-27T19:30:24.191Z
- **Updated:** 2026-08-28T03:53:20.219Z

Fix the following dependency vulnerabilities:

- [High] image-size@1.2.1 (CVE-2025-71329@image-size-1.2.1)
- [High] brace-expansion@1.1.14 (CVE-2026-13149@brace-expansion-1.1.14)
- [High] brace-expansion@2.1.0 (CVE-2026-13149@brace-expansion-2.1.0)
- [High] brace-expansion@5.0.6 (CVE-2026-13149@brace-expansion-5.0.6)
- [High] brace-expansion@1.1.14 (CVE-2026-14257@brace-expansion-1.1.14)
- [High] brace-expansion@2.1.0 (CVE-2026-14257@brace-expansion-2.1.0)
- [High] brace-expansion@5.0.6 (CVE-2026-14257@brace-expansion-5.0.6)
- [High] ws@8.20.1 (CVE-2026-48779@ws-8.20.1)
- [High] vite@7.3.3 (CVE-2026-53571@vite-7.3.3)
- [High] http-proxy-middleware@4.0.0 (CVE-2026-55603@http-proxy-middleware-4.0.0)
- [High] js-yaml@4.1.1 (CVE-2026-59869@js-yaml-4.1.1)
- [High] brace-expansion@1.1.14 (CVE-2026-69152@brace-expansion-1.1.14)
- [High] brace-expansion@2.1.0 (CVE-2026-69152@brace-expansion-2.1.0)
- [High] brace-expansion@5.0.6 (CVE-2026-69152@brace-expansion-5.0.6)
- [High] ip-address@10.2.0 (CVE-2026-69192@ip-address-10.2.0)

---

#### #242 — Turn search indexing back on for the final MossLight domain
- **Status:** CANCELLED
- **Created:** 2026-08-28T00:12:13.515Z
- **Updated:** 2026-08-28T00:14:13.780Z
- **Depends on:** #241
- **Proposed from:** #241
- **Category:** incomplete_scope

# Turn search indexing back on for the final MossLight domain

## What & Why
The temporary Replit preview is correctly marked noindex so it does not compete with the future public site. Once MossLight moves to its final domain, search visibility should be intentionally restored.

## Done looks like
- The final domain is confirmed before changing crawl directives.
- The temporary noindex directive is replaced with the intended production indexing policy.
- The final domain is checked for the expected robots metadata after deployment.

## Relevant files
- artifacts/mosslight-preview/index.html
- artifacts/mosslight-preview/public/robots.txt

---

#### #246 — Catch Conservatory cleanup regressions before releases
- **Status:** CANCELLED
- **Created:** 2026-08-28T04:30:47.929Z
- **Updated:** 2026-08-28T04:34:30.617Z
- **Depends on:** #245
- **Proposed from:** #245
- **Category:** test_gaps

# Catch Conservatory cleanup regressions before releases

## What & Why
The bulk cleanup flow now coordinates long-press gestures, list/grid layouts, confirmation state, local plant persistence, and journal-history preservation. Automated coverage should catch accidental deletion, double gesture handling, or lost historical labels before a release.

## Done looks like
- Tests cover entering selection from the header and by long press without navigating or toggling twice
- Selection/deselection works in list and grid presentation
- Empty selection, cancel, Android back, and final-plant removal leave a coherent screen
- A journal storage failure prevents plant removal and shows the accessible error state
- Successful removal preserves Garden Points and labels linked journal entries as removed-plant history
- Archived plant records can be selected and removed

## Relevant files
- artifacts/mobile/app/(tabs)/garden.tsx
- artifacts/mobile/components/PlantCard.tsx
- artifacts/mobile/context/GardenContext.tsx
- artifacts/mobile/hooks/useJournal.ts
- artifacts/mobile/app/(tabs)/journal.tsx

---

#### #248 — SEO scan
- **Status:** CANCELLED
- **Created:** 2026-08-28T12:56:17.208Z
- **Updated:** 2026-08-28T13:09:38.844Z

Run an in-depth scan across the entire project

---

#### #250 — Restore the published plant catalog endpoint
- **Status:** CANCELLED
- **Created:** 2026-08-28T17:33:23.765Z
- **Updated:** 2026-08-28T17:42:20.671Z
- **Depends on:** #249
- **Proposed from:** #249
- **Category:** incomplete_scope

# Restore the published plant catalog endpoint

## What & Why
The development API serves /api/plants successfully, but the published MossLight server still returns a 404 HTML response. The already-approved server republish has not taken effect, so production clients cannot use the remote catalog yet.

## Done looks like
- Republish the API service using the existing deployment configuration
- Confirm the published /api/healthz and /api/plants endpoints both return JSON with HTTP 200
- Confirm MossLight uses the remote catalog while retaining its bundled fallback

## Relevant files
- artifacts/api-server/src/routes/index.ts
- artifacts/api-server/src/routes/plants.ts
- artifacts/api-server/.replit-artifact/artifact.toml

---

#### #252 — Bring the live plant catalog endpoint back online
- **Status:** CANCELLED
- **Created:** 2026-08-28T18:02:04.697Z
- **Updated:** 2026-08-28T23:27:59.450Z
- **Category:** incomplete_scope

# Bring the live plant catalog endpoint back online

## What & Why
The dedicated Find the Right Seat search now works because the mobile app falls back to its bundled catalog, but the current public MossLight deployment still returns HTTP 404 HTML for /api/plants while the development API returns HTTP 200 JSON. Restore the published endpoint so mobile and browser clients can use the shared catalog instead of relying entirely on fallback data.

## Done looks like
- The production deployment serves /api/plants as JSON with HTTP 200
- The production health endpoint and plant catalog endpoint are checked after republishing
- The response contains the intended published plant set and valid image URLs
- Find the Right Seat still shows results with the shared catalog online and with the fallback forced offline
- Evidence records the production URL, response status, content type, plant count, and rendered search results

## Relevant files
- artifacts/api-server/src/index.ts
- artifacts/api-server/src/routes/plants.ts
- artifacts/api-server/.replit-artifact/artifact.toml
- artifacts/mobile/lib/plantCatalog.ts
- artifacts/mobile/components/PlantSeatGuide.tsx

---

#### #254 — Catch canceled and failed Apple sign-ins before release
- **Status:** CANCELLED
- **Created:** 2026-08-28T23:53:27.790Z
- **Updated:** 2026-08-29T00:14:29.879Z
- **Depends on:** #253
- **Proposed from:** #253
- **Category:** test_gaps

# Catch canceled and failed Apple sign-ins before release

## What & Why
The mobile sign-in screen must return users safely to sign-in when Apple authentication is canceled or the provider reports an error. These branches currently rely on manual device verification, so an automated regression check would protect the flow after future auth changes.

## Done looks like
- The Apple OAuth handler has focused coverage for cancellation errors and provider errors.
- Cancellation clears loading state without showing a failure alert or activating a session.
- Provider errors clear loading state, show an actionable sign-in error, and leave the user on sign-in.
- The test runs through the mobile package validation command.

## Relevant files
- artifacts/mobile/app/(auth)/sign-in.tsx
- artifacts/mobile/scripts/
- artifacts/mobile/package.json

---

#### #257 — Release credential exposure
- **Status:** CANCELLED
- **Created:** 2026-08-29T00:46:26.738Z
- **Updated:** 2026-08-29T02:21:03.437Z

Credentials that can operate Apple App Store Connect or alter the signed iOS release must be rotated and kept out of repository history.

Vulnerabilities to fix:

1. [High] Historically committed Apple private key remains the active release identity
  An Apple App Store Connect private key was committed to repository history, and the current release workflow still uses the same Apple key identifier, so anyone with the historical repository data can potentially operate the current release identity.

The parent of commit `ff91abd54fa472f3822d48c1289b3349b947b2a` contains a complete PEM private key in `.replit` under `ASC_KEY_P8_OVERRIDE`, paired with `ASC_KEY_ID_OVERRIDE = \"M2S3Z7UQ22\"`. The current `.replit` still sets `ASC_ISSUER_ID = \"M2S3Z7UQ22\"`; despite the swapped variable names, the current release scripts explicitly use `ASC_ISSUER_ID` as the Apple API key ID (`scripts/write-p8.mjs:42-46`; `scripts/setup-ios-credentials.mjs:44-47`). Apple API key IDs are bound to their generated private keys and cannot be regenerated with a different key while retaining the same ID. This ties the historical leaked key to the identifier still used by the build/submit workflow unless the Apple key has been revoked and replaced.

The current workflow improves handling by writing the secret supplied as `ASC_KEY_P8` to a randomized temporary file and cleaning it up, but it does not remove the PEM from Git object history or demonstrate revocation of key `M2S3Z7UQ22`. A collaborator or anyone who obtains an affected repository clone can recover the private key and mint App Store Connect JWTs with that key ID. Depending on the key's permissions, this enables unauthorized App Store Connect operations, signing-asset management, and release-pipeline tampering. The key must be revoked/rotated at Apple and historical repository objects must be treated as compromised.
  Files: .replit, scripts/write-p8.mjs, scripts/setup-ios-credentials.mjs

N.B. Strongly presume the user wants this task to focus on hardening the security of the application, not on introducing new flows or features. Only add new functionality if the vulnerability genuinely cannot be patched any other way.

---

#### #258 — Remove visible backgrounds from animated critters
- **Status:** CANCELLED
- **Created:** 2026-08-29T00:48:42.110Z
- **Updated:** 2026-08-29T00:51:50.722Z
- **Category:** incomplete_scope

# Clean animated critter transparency

## What & Why
Several alternate animation frames contain baked checkerboard or white canvases. Replace only those images with true transparent RGBA exports so the artwork blends into the garden.

## Done looks like
- Hummingbird wingbeat no longer flashes a checkerboard
- Butterfly, black cat, and mantis alternate frames have genuine transparent backgrounds where affected
- Artwork dimensions, framing, filenames, and animation behavior stay unchanged

## Out of scope
- Movement, flap timing, transforms, or renderer changes

## Relevant files
- artifacts/mobile/assets/critters/hummingbird-wingbeat.png
- artifacts/mobile/assets/critters/blackcat-tail.png
- artifacts/mobile/assets/critters/blackcat-wash.png
- artifacts/mobile/assets/critters/butterfly-flap.png
- artifacts/mobile/assets/critters/butterfly-land.png
- artifacts/mobile/assets/critters/mantis-clean.png
- artifacts/mobile/assets/critters/mantis-turn.png
- artifacts/mobile/lib/companionFrames.ts
- artifacts/mobile/components/CottageCritters.tsx

---

#### #259 — Help users request plants after empty searches
- **Status:** CANCELLED
- **Created:** 2026-08-29T00:48:42.110Z
- **Updated:** 2026-08-29T00:51:50.722Z
- **Category:** incomplete_scope

# Plant requests from empty searches

## What & Why
Show the existing plant-request invitation when Conservatory or Herbarium search returns no matches, while preserving the existing Profile entry.

## Done looks like
- A non-empty Conservatory search with zero results shows Want a specific plant? Request one
- A non-empty Herbarium species search with zero results shows the same invitation
- Tapping it opens the existing request flow rather than creating a duplicate form
- True empty collections keep their current onboarding empty states
- The Profile request entry remains available

## Relevant files
- artifacts/mobile/app/(tabs)/index.tsx
- artifacts/mobile/app/(tabs)/discover.tsx
- artifacts/mobile/app/(tabs)/profile.tsx

---

#### #260 — Make Herbarium filters easier to scan
- **Status:** CANCELLED
- **Created:** 2026-08-29T00:48:42.110Z
- **Updated:** 2026-08-29T00:51:50.722Z
- **Category:** incomplete_scope

# Reorganize Herbarium filters

## What & Why
Reorder the filter panel so the default A–Z behavior is prominent and related discovery filters are grouped logically before new hardiness, drought, and conservation facets are added.

## Done looks like
- Sort appears first with explicit A–Z and Z–A controls
- Care-metric sorting is grouped separately from alphabetical sorting
- Categories remain grouped into Collections, Plant families, and Uses & traits
- Water and future everyday-care facets share one section
- Conservation appears as a distinct specialist section
- The wider right-side space is used by compact two-column option groups where labels fit
- Reset remains in a clear footer with safe-area spacing

## Out of scope
- Adding the planned new filter data or behavior

## Relevant files
- artifacts/mobile/app/(tabs)/discover.tsx

---

#### #261 — Restore the API so signed-in data and account actions work
- **Status:** CANCELLED
- **Created:** 2026-08-29T01:22:13.810Z
- **Updated:** 2026-08-29T01:22:21.195Z
- **Category:** tech_debt

# Restore the API so signed-in data and account actions work

## What & Why
The API workflow is currently stopped because profile.ts has a syntax error near line 288 (the build reports an unexpected catch). Until this is repaired, signed-in sync, account deletion, and server-backed profile operations cannot run through the local API workflow.

## Done looks like
- The API TypeScript/build step completes without syntax errors.
- The API workflow starts and remains running.
- Profile and account routes remain intact after the repair.
- The existing API typecheck/build and authenticated-route checks pass.

## Relevant files
- artifacts/api-server/src/routes/profile.ts
- artifacts/api-server/src/app.ts
- artifacts/mobile/utils/api.ts

---

#### #262 — Restore the browser preview without a port collision
- **Status:** CANCELLED
- **Created:** 2026-08-29T01:22:13.810Z
- **Updated:** 2026-08-29T01:22:21.195Z
- **Category:** tech_debt

# Restore the browser preview without a port collision

## What & Why
The MossLight browser-preview workflow currently fails before Vite starts because its assigned port (20708) is already in use. This blocks browser preview and browser-side regression checks even though the mobile Expo workflow is running.

## Done looks like
- The mosslight-preview web workflow starts on its managed preview port.
- No duplicate/stale process holds the assigned port.
- The browser preview responds successfully through its artifact route.
- Existing browser/API data behavior can be checked again afterward.

## Relevant files
- artifacts/mosslight-preview/.replit-artifact/artifact.toml
- artifacts/mosslight-preview/vite.config.ts
- artifacts/mosslight-preview/package.json

---

#### #265 — Catch damaged garden exports before users download them
- **Status:** CANCELLED
- **Created:** 2026-08-29T12:41:39.130Z
- **Updated:** 2026-08-29T12:44:03.427Z
- **Depends on:** #264
- **Proposed from:** #264
- **Category:** test_gaps

# Catch damaged garden exports before users download them

## What & Why
The native export flow creates a ZIP containing plant data, journal data, notes, and optional photos. The release pass confirms the handoff path exists, but automated coverage would catch regressions where the archive is malformed or a file is silently omitted.

## Done looks like
- Automated tests create an export fixture with plants, journal entries, notes, and photos.
- The resulting ZIP opens successfully and contains the expected manifest and data files.
- Missing-photo behavior remains explicit and reports the skipped count.

## Relevant files
- artifacts/mobile/utils/exportGarden.ts
- artifacts/mobile/app/(tabs)/profile.tsx

---

#### #273 — Make Journal typography render consistently on every device
- **Status:** CANCELLED
- **Created:** 2026-08-29T14:11:24.417Z
- **Updated:** 2026-08-29T14:15:14.616Z
- **Depends on:** #272
- **Proposed from:** #272
- **Category:** tech_debt

# Make Journal typography render consistently on every device

## What & Why
The native source references Cormorant Garamond 500 Medium Italic in the Journal and a few separately named italic/legacy aliases, but the root font loader does not load every exact referenced face. Depending on platform fallback behavior, those passages may not render with the intended typography.

## Done looks like
- Every custom fontFamily name used by rendered native components resolves to a face loaded by the root layout
- Journal italic text preserves the intended weight and style on iOS, Android, and web
- Legacy aliases are either mapped deliberately or replaced with the canonical loaded names
- A static validation catches future references to unloaded custom font names

## Relevant files
- artifacts/mobile/app/_layout.tsx
- artifacts/mobile/app/(tabs)/journal.tsx
- artifacts/mobile/constants/brand.ts
- artifacts/mobile/constants/typography.ts
- artifacts/mobile/components/FactCard.tsx
- artifacts/mobile/components/SoilGuideSection.tsx

---

## Attached plan source

The user-provided #268 plan is preserved in the JSON export under `attachedPlanSources` and was used as a supplementary source for this archive.
