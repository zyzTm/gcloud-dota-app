# Dota 2 App using Firebase and Google Cloud

This is a pet project that born with the idea of having a nice subject to go through some Live Coding sessions. We are getting data from [DotaBuff](https://dotabuff.com) website to have more info about the heroes and latest matches.

That info is going to be used on different frontends to build different features, some of them are :

- Show which heroes are good/bad against a given hero.
- Show a rank of best heroes
- Build both match teams and get recommendations while heroes are being picked.

Here you can see some screenshots of the mobile app, available on this repository, built with Flutter:

<img src="https://github.com/alvarowolfx/gcloud-flutter-dota-app/raw/master/.images/screenshot01.jpeg" width="20%" height="20%" /> <img src="https://github.com/alvarowolfx/gcloud-flutter-dota-app/raw/master/.images/screenshot02.jpeg" width="20%" height="20%" /> <img src="https://github.com/alvarowolfx/gcloud-flutter-dota-app/raw/master/.images/screenshot03.jpeg" width="20%" height="20%" />

Also we built a Voice integration that will allow users to access some of those same features from the app. The voice integration was built using Dialogflow and the agent data can be found on the `dialogflow` folder and the HTTP fullfilment is inside the `functions` folder.

[Here you can a demo of the Voice integration (PT-BR)](https://twitter.com/alvaroviebrantz/status/1258213959787786246?s=20)

The live codings session happens in Portuguese (PT-BR) and you can follow on my Youtube/Twitch channels.

- [Alvaro Viebrantz on Youtube](https://youtube.com/alvaroviebrantz)
- [Alvaro Viebrantz on Twitch](https://twitch.tv/alvaroviebrantz)

## Outstanding External Collaborators

- [@omurilo](https://github.com/omurilo) Created a version of the mobile using React Native - [Link](https://twitter.com/alvaroviebrantz/status/1258213959787786246?s=20gcloud-react-native-dota-app)

## Table of Contents

- [Getting Started](#getting-started)
  - [Node Setup](#node-setup)
  - [Firebase Setup](#firebase-setup)
  - [Google Cloud Setup](#google-cloud-tools-and-project)
  - [Flutter Setup](#google-cloud-tools-and-project)
  - [Building & Running](#building-&-running-the-project)
  - [Dialogflow Setup](#dialogflow-setup)
- Project Structure
  - Cloud Function - `functions` folder
    - `scheduledFetchDotaBuffHeroes` : Scheduled PubSub handler that fetch all dota heroes and queue them up to be processed by `fetchDotaBuffHeroById`.
    - `fetchDotaBuffHeroById`: PubSub consumer that fetchs a given Dota hero data from DotaBuff and save data on Firebase.
    - `dialogflowFirebaseFulfillment`: Handle Dialogflow/Google Assistant Interaction
  - Go API - `go-api` folder
    - Hero Recommendation API.
    - We separated all the logic to recommend heroes to pick depending on the heroes that the enemy got and what you team already picked. We use two methods :
      - Intersection between all heroes that are good against the enemies heroes
      - Top heroes by sorted by advantage against all heroes picked by the enemies
    - API Always return at least 3 recommended heroes.
  - Flutter App - `flutter_dota_app` folder
    - Mobile App build with Flutter
  - Dialogflow Agent - `dialogflow` folder
    - All intents and entities used to build the voice integration.

## Getting Started

### Node Setup

- Install the latest LTS version of [Node.js](https://nodejs.org/) (which includes npm). An easy way to do so is with `nvm`. (Mac and Linux: [here](https://github.com/creationix/nvm), Windows: [here](https://github.com/coreybutler/nvm-windows))

```shell
nvm install --lts
```

### Firebase Setup

- Install the [Firebase CLI](https://firebase.google.com/docs/cli) via `npm`. The following command enables the globally available `firebase` command:

```shell
npm install -g firebase-tools
```

- After installing the CLI, you must authenticate. Then you can confirm authentication by listing your Firebase projects. Sign into Firebase using your Google account by running the following command:

```shell
firebase login
```

- Test that the CLI is properly installed and accessing your account by listing your Firebase projects. Run the following command:

```shell
firebase projects:list
```

- Test that the CLI is properly installed and accessing your account by listing your Firebase projects. Run the following command:

```shell
firebase projects:list
```

### Google Cloud Tools and Project

- Install gcloud [CLI](https://cloud.google.com/sdk/install)
- Authenticate with Google Cloud:
  - `gcloud auth login`
- Create cloud project — choose your unique project name:
  - `gcloud projects create YOUR_PROJECT_NAME`
- Set current project
  - `gcloud config set project YOUR_PROJECT_NAME`
- Set current project
  - `firebase use YOUR_PROJECT_NAME`

### Flutter Setup

- Follow the guide on their [website](https://flutter.dev/docs/get-started/install).
- Run the following command to make sure it's all good.

```shell
flutter doctor
```

## Building & Running the project

- Make sure you have the latest packages (after you pull): `npm install`
- Deploy all the function from the `functions` directory.
  - There are deploy scripts on the `package.json` file.
- To run the app, run `flutter run` on the `flutter_dota_app` folder

## Dialogflow Setup

1. Create a [Dialogflow Agent](https://console.dialogflow.com/).
2. Go to **Settings** ⚙ > **Export and Import** > **Restore from zip** using the `dialogflow/DotaAppAgent.zip` in this directory.
3. `cd` to the `functions` directory
4. Run `firebase deploy --only functions:dialogflowFulfillment`
5. Back in Dialogflow Console > **Fulfullment** > **Enable** Webhook.
   - Paste the URL from the Firebase Console’s Trigger column under the **Functions > Dashboard** tab into the **URL** field > **Save**.
