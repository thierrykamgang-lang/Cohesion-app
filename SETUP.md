# Cohesion — Setup

## What's built so far
- Full project scaffold (Gradle, Kotlin, Jetpack Compose, Material 3)
- Data models for **all 8 modules**: Members, Board/Committees, Dues, Tontines,
  Life Insurance, Cash Ledger, Votes/Surveys, Projects/Events/Meetings, Chat/Notifications
  (`data/model/`) — so every future module plugs into a consistent schema.
- **Auth** — email/password login + sign-up via Firebase Auth. New sign-ups are
  created as a `Member` with status `PENDING_APPROVAL` until an admin approves them.
- **Members management** — fully working: list screen, add/edit screen, live
  Firestore sync, role assignment (Member / Committee Lead / Executive Board / Admin).
- **Chat** — intentionally NOT built as in-app messaging. Instead it's a single
  screen that deep-links out to the association's WhatsApp group (`ChatScreen.kt`).
  An admin pastes the WhatsApp invite link once (stored on the `Association` doc);
  after that, members just tap "Open WhatsApp Group." Zero chat infra to maintain.
- Bottom navigation bar (Members / Chat) that appears once logged in.
- App theme matching your purple/gold brand.

## What's not built yet
Dues payments (Stripe), tontines, insurance, cash ledger, votes/surveys,
projects/events/meetings, push notification UI, French localization, and an
admin approval screen for pending members. We'll add these module by module
the same way Members and Chat were built.

## Note on the WhatsApp chat approach
Right now *any* logged-in member can paste/overwrite the group link (there's a
`TODO` in `ChatViewModel.kt` marking where to restrict this to admins once role
lookups are wired in — worth doing before real users touch this).

## To run this yourself

1. **Install Android Studio** (free): https://developer.android.com/studio
2. **Open the project**: File → Open → select the `GravitySerenity` folder
3. **Create a Firebase project** (free tier is enough for now):
   - Go to https://console.firebase.google.com → Add project
   - Add an Android app with package name `ca.gravityserenity.app`
   - Download the generated `google-services.json`
   - Drop it into `GravitySerenity/app/google-services.json` (same folder as `build.gradle.kts` for the app module)
4. **Enable Firestore**: In Firebase console → Build → Firestore Database → Create database (start in test mode for now, we'll lock down rules before launch)
5. **Enable Authentication**: Firebase console → Build → Authentication → get started (we'll wire up email/password or phone auth next)
6. Click **Run ▶** in Android Studio with an emulator or physical device connected

## Firestore data structure
```
associations/{associationId}/
  members/{memberId}
  boardPositions/{id}
  committees/{id}
  dues/{id}
  tontines/{id}
  ...
```
Everything is scoped under an association so this could later support multiple
associations in one deployment if needed.

## Next module to build
Your call — I'd suggest **Auth + login** next since every other screen needs
to know who's logged in, then whichever feature matters most to you day-to-day
(Dues is a common next pick since it ties to payments).
