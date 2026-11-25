# Maka Nana Shisanyama: Firebase setup

Customers order. Orders go to the Maka Nana portal. Only Maka Nana staff can confirm payment, mark done or cancel.

## 1. Create the project
1. Go to https://console.firebase.google.com and click **Add project**. Name it `maka-nana`.
2. **Build > Authentication > Get started > Sign-in method**. Turn on **Anonymous** and **Email/Password**.
3. **Authentication > Users > Add user**. Add Maka Nana's email and a strong password. Do this first.
4. **Build > Firestore Database > Create database**. Choose production mode. Pick `africa-south1` (Johannesburg) if offered.

## 2. Connect the app
1. **Project settings (gear) > Your apps > Web (`</>`)**. Register the app and copy the `firebaseConfig`.
2. Open `public/index.html`. Find `FIREBASE_CONFIG` near the bottom and paste your values.
3. In the same file, set `ADMIN_EMAILS` to Maka Nana's staff email.
4. Open `firestore.rules` and make sure the same staff emails are listed (Maka Nana and Sizwe).

## 3. Publish the rules
Firestore > **Rules** tab: paste the contents of `firestore.rules` and click **Publish**.

## 4. Deploy
Firebase Hosting:
```
npm install -g firebase-tools
firebase login
firebase init hosting firestore   # use existing project, keep public/ and firestore.rules, do NOT overwrite
firebase deploy
```
Or upload the `public` folder to Vercel (it is a plain static site).

## 5. Test
1. Open the site on a phone. Choose a meal, pick "I'm at the shisanyama", send the order.
2. On another device tap **Staff login** at the bottom, sign in, then open the portal.
3. The order appears with a beep. Tap **Confirm payment**. The customer screen updates.
4. Portal > **Menu stock** > mark an item sold out. Customers see it greyed out.

## Security notes
- Staff access is checked by the Firestore rules, not by the page. Keep `ADMIN_EMAILS` and the rules email the same.
- Keep the staff password private. Add another staff email by adding it to both lists.
- Turn on **App Check** (Build > App Check) before launch to cut spam orders.
- Replace the placeholder menu prices in `public/index.html` (the `MENU` object).
