# End-to-end encrypted, HIPAA-compliant JavaScript demo chat app for Firebase.

You can reuse this sample in any your projects to protect user data, documents, images using Virgil's end-to-end encryption [HIPAA whitepaper](https://virgilsecurity.com/wp-content/uploads/2018/07/Firebase-HIPAA-Chat-Whitepaper-Virgil-Security.pdf).

**This Demo is based on [Virgil E3Kit JS SDK](https://github.com/VirgilSecurity/virgil-e3kit-js).**

## Prerequisites

* [node v10](https://nodejs.org/en/download) or later
* [npm](https://www.npmjs.com/get-npm) or yarn

## Clone JavaScript project
```bash
git clone https://github.com/VirgilSecurity/demo-firebase-js
cd demo-firebase-js
```

## Connect your Virgil and Firebase accounts
To connect your Virgil and Firebase accounts for implementing end-to-end encryption to deploy a Firebase function that gives out Virgil JWT tokens for your authenticated users.

To deploy the function, head over to our GitHub repo and follow the instructions in README:

* **[Follow instructions here](https://github.com/VirgilSecurity/e3kit-firebase-func)**

### Configure Authentication

* Select the **Authentication** panel and then click the **Sign In Method** tab.
* Choose your authentication method and turn on the **Done** switch, then follow instructions and click **Save**.

### Configure Cloud Firestore

* Let's also set up a Firestore database for the sample apps: select the **Database** panel, select **Cloud Firestore** click **Create database** under Firestore, choose **Start in test mode** and click **Enable**.
* Once the database is created, click on the **Rules** tab, click **Edit rules** and paste:
  ```
  service cloud.firestore {
    match /databases/{database}/documents {
      match /{document=**} {
        allow read, write: if request.auth.uid != null;
      }
    }
  }
  ```
* Click **PUBLISH**.

> You only need to do this once - if you did it already earlier or for your Android or iOS apps, don't need to do it again.

## Add your Firebase project config to app

* Go to the Firebase console -> your project's page in Firebase console, click the **gear icon** -> **Project settings**
* Click **Add app** and choose **"</> Add Firebase to your web app"**
* Copy **only this part** to the clipboard:
  ```
    var firebaseConfig = {
      apiKey: "...",
      authDomain: "...",
      databaseURL: "...",
      projectId: "...",
      storageBucket: "...",
      messagingSenderId: "..."
    };
  ```
* **Replace the copied block** in your `src/firebase.ts` file.

## Test it

* **Update dependencies, build & run**
  ```
  npm install
  npm run start
  ```

> In case you receive a message like `warning found n vulnerabilities` printed in the console after running the `npm install`, there is a potential security vulnerability in one of the demo's dependencies. Don't worry, this is a normal occurrence and in the majority of cases, is fixed by updating the packages. To install any updates, run the command `npm audit fix`. If some of the vulnerabilities persist after the update, check the results of the `npm audit` to see a detailed report. The report includes instructions on how to act on this information.

* **Browse to http://localhost:1234**

* Start a **second incognito or browser window** to have 2 chat apps running with 2 different users

> Remember, the app deletes messages right after delivery (it's a HIPAA requirement to meet the conduit exception). If you want to see encrypted messages in your Firestore database, run only 1 browser instance, send a message to your chat partner and check Firestore DB's contents before opening the other user's app to receive the message. If you don't want to implement this behavior in your own app, you can remove it from this sample [here](https://github.com/VirgilSecurity/demo-firebase-js/blob/d263f0ddd4f92f51ee2a925cdffd32a19a0387ae/src/models/MessageListModel.ts#L34).
