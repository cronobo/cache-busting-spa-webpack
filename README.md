# cache-busting-spa-webpack

> This repository illustrates how to configure correctly static file serving to make caching work correctly for single page apps that use content fingerprinting (Explain)

> This repository deploys the default VueJS app to Firebase (using Firebase's static hosting free tier).

## Setup

You need Firebase to deploy this application.
Go to your Firebase console and create a new project.
Install locally the firebase cli if you don't already have it.

```
npm install -g firebase-tools
```

Associate the Firebase project you've just created with this repo, and you're ready to deploy.

```
firebase use --add <your-firebase-project-name>
```

## Important

Firefox and Chrome will perform a hot reload if you click the reload button.

To make sure you are not clearing your browser's cache unintentionally (which would defeat the purpose of this demo), simply click in the url and hit `Enter`.

## Deploy

First, checkout branch `step-1-cache-everything`, build the project and deploy

```
git checkout step-1-cache-everything
npm run build && firebase deploy
```

To observe the behavior of your browser cache, and how your SPA is **NOT updating immediately**:

1. Navigate a first time to your deployed app (URL should be `https://<your-firebase-project-name>.firebaseapp.com`)
2. In `./src/components/HelloWorld.vue`, modify the `msg` variable (default value: `This is version 1`) to some other value
3. Run `firebase deploy` and navigate again to your deployed app
4. Observe that the message is still `This is version 1` and not the new version.
5. Perform a forced reload (`Ctrl + F5`) and observe the message is updated this time

> Why ? Go to Medium article

Then, checkout branch `step-2-no-cache-on-root`, re-build, re-deploy.

```
git checkout step-2-no-cache-on-root
npm run build && firebase deploy
```

Force your browser to update its cache by navigating again to your deployed app and performing a forced reload (`Ctrl + F5`).

Then performs the same steps as above. Observe that this time the msg is updated immediately without needing to perform a forced reload.


## Run locally

You can run this project locally with:

```
npm run dev
```

Remember that browsers don't keep cache for localhost URLs, so the demonstration above only works if you deploy online.
