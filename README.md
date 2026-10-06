# demo-login

The website for [Passkey Demo](https://github.com/aman-dhakar-191/passkey-provider-android) (the test app in
`passkey-provider-android`). It does two things:

- **`public/.well-known/assetlinks.json`** is what lets Passkey Vault trust the demo app. Passkey Vault only lets an
  app use a site's passkeys if the site publishes this file at the **root** of its address and names the app
  and its signing certificate. No redirects: Android and Passkey Vault both refuse them.
- **`public/index.html`** is a login page that asks for passkeys, like the login pages some apps show inside
  the app. It uses whatever address it is served from as its site.

Hosted on Firebase Hosting, which gives the site its own address (`<project-id>.web.app`) with the file at the root.

## Deploy

1. In the [Firebase console](https://console.firebase.google.com/), create a project (the free plan is enough) and
   note its **project ID**. Your site will be `https://<project-id>.web.app`.
2. On a computer with Node.js: `npm install -g firebase-tools`, then `firebase login`.
3. In this folder: `firebase use --add` (pick the project), then `firebase deploy --only hosting`.

(To deploy from GitHub instead, run `firebase init hosting:github` once; it adds the workflow and the secret.)

`firebase.json` deliberately does **not** ignore files starting with a dot. Firebase's default settings skip them,
which would leave out the `.well-known` folder, and the site would look fine but Android would find nothing.

## Check

- `https://<project-id>.web.app/.well-known/assetlinks.json` shows the JSON with no redirect.
- Passkey Demo's **Check site setup** button reports every line as passed (it uses the site name it was built with;
  see the demo release's `rp_id` setting).

## If the demo app is re-signed

`sha256_cert_fingerprints` must match the key the demo APK is signed with. The *Demo release* workflow in
`passkey-provider-android` attaches the right `assetlinks.json` to every demo release.
