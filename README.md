# It Takes Two

A private, two-person messaging app for Android, built for couples.

I wanted a chat app without ads and with control over my own data to replace [Between](https://between.us/?lang=en) as my daily messenger. So I built my own: messages and photos live in a Supabase project I own and the app works offline from a local database.

### Demo
https://github.com/user-attachments/assets/035c455c-2d2b-4686-8185-117aef3b4842

## Features
- **Account pairing**: sign up, sign in, and link with your partner by email
- **Realtime chat**: instant messages over Supabase WebSocket channels, with read receipts and an unread badge
- **Photo sharing**: send up to 10 images per message using the system photo picker
- **Shared gallery**: browse all shared media and view it full screen with pinch-to-zoom
- **Profiles**:upload an avatar and see your own and your partner's profile changes in real time 
- **Push notifications**: a new message triggers a Supabase Edge Function that sends a push notification via Firebase Cloud Messaging  
- **Offline-first**: past messages and media persist locally and are accessible even without an internet connection

## Tech Stack

- **UI:** Kotlin, Jetpack Compose, Material 3, Navigation 3, Glide for image loading
- **Architecture:** Hilt for dependency injection, Coroutines and Flow
- **Local data:** Room, Proto DataStore
- **Backend:** Supabase (Auth, Postgrest, Realtime, Storage) and Firebase Cloud Messaging, Ktor for file downloads

## Build & Run

### Minimum requirements

- Android Studio Panda 4 (2025.3.4)
- Android 15 (API 35) or higher

### Run the app

1. Clone the repository and open it in Android Studio.
2. Add your Firebase `google-services.json` to the `app/` module.
3. Create a `secrets.properties` file in the project root containing your `SUPABASE_URL` and `SUPABASE_PUBLISHABLE_KEY`. Keep this file out of version control.
4. Set up the Supabase backend:
   - Tables: `users`, `pairings`, `chat_messages`, `message_attachments`
   - Storage buckets: `avatars` and `messages`
   - Realtime: private channels for chat messages and user updates, with row-level security so only paired users can read each other's data
5. Run the app on a device or emulator.

## Architecture

The app uses MVVM structure. Screens observe ViewModels, ViewModels talk to repositories, and repositories coordinate the local database with the Supabase backend. Room is the source of truth for the UI.

### Data flow

```
Compose UI  →  ViewModel (StateFlow UI state)  →  Repository  →  Room DAO  ⇄  Network data source (Supabase)
```

### Project structure

```
app/src/main/java/me/linhvo/ittakestwo/
├── auth/           Sign in and sign up
├── chat/           Chat screen, message bubbles, input bar
├── database/       Room database, DAOs, entities, data sync
├── datastore/      Proto DataStore configuration
├── gallery/        Shared media grid and full-screen viewer
├── home/           Home screen
├── navigation/     Routes and bottom navigation bar
├── network/        Supabase data sources and network models
├── notification/   Firebase messaging service
├── profile/        Profile and add-partner dialogs
├── repository/     Auth, User, Pairing, Chat, and Storage repositories
├── settings/       Settings screen (placeholder)
└── ui/             Theme, typography, shared components
```

### Supabase schema 
<img width="1494" height="771" alt="schema" src="https://github.com/user-attachments/assets/e57e7da9-5a45-45e4-9ded-56ef9a8a3da0" />


## Roadmap

- [ ] Message encryption
- [ ] Message search
- [ ] Shared calendar and important dates
- [ ] Edit shared background for the home screen
- [ ] Handling pairing removal
- [ ] Settings screen
- [ ] Shared to-do lists