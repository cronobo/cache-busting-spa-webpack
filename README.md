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

## Observe the issue

### Step 1 - deploy

Checkout branch `step-1-cache-everything`, build the project and deploy

```
git checkout step-1-cache-everything
npm run build && firebase deploy
```

Open your browser -> DevTools -> Network panel, then navigate a first time to your deployed app.

(URL should be `https://<your-firebase-project-name>.firebaseapp.com`).

![step 1 loads all files](step_1_initial_load.png)

As you can see, all files are being loaded with return code `200`.

### Step 2 - observe caching at work

In `./src/components/HelloWorld.vue`, modify the `msg` variable (default value: `This is version 1`) to some other value

Run
```
npm run build && firebase deploy
```
and navigate again to your app (click `Enter` inside URL bar to avoid unwanted forced reloads).

You should observe that the message has not changed, and is still `This is version 1`, despite the update.

![](step_2_cache_is_not_good.png)

In the network tabs, you can see that all files are displayed in grey, meaning their cached version was reused.

If you perform a forced reload (`Ctrl + F5`), you can observe that this time the message gets updated.

### Step 3 - fix caching for immediate update

Checkout branch `step-2-no-cache-on-root` and redeploy.
In this branch, `index.html` caching setting was changed to `cache-control:no-cache` (see `firebase.json`)

```
git checkout step-2-no-cache-on-root
npm run build && firebase deploy
```

Force your browser to update its cache by navigating again to your deployed app and performing a forced reload (`Ctrl + F5`).

Then, change msg, re-build and re-deploy, and navigate to the page **without forced reload** (Click enter in URL bar).

You should observe this time that your application was correctly updated, and the new message is displayed. Issue is fixed.
In the networks tab, you can see that `index.html` and two of the application files (`app.xxxx.js`) were updated, meaning your cache is properly configured for immediate updates.

![cache is now ok](step_4_cache_configured.png)

## Run locally

You can run this project locally with:

```
npm run dev
```

Remember that browsers don't keep cache for localhost URLs, so the demonstration above only works if you deploy online.
