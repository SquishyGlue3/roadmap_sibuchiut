# roadmap_sibuchiut

Progress and notes are saved automatically in the current browser. To sync them between devices, configure the optional Firebase integration in `index.html`:

1. Create a Firebase project on the **Spark (no-cost)** plan. Cloud Firestore's current free quota includes 1 GiB stored data, 20,000 document writes/day, and 50,000 document reads/day. Do not upgrade to a billing plan for this setup.
2. In **Authentication**, enable the **Email/Password** provider. In its settings, add `squishyglue3.github.io` to the authorized domains.
3. Create a **Cloud Firestore** database. In its **Rules** tab, publish these rules so each signed-in user can access only their own progress:

   ```text
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /userStates/{userId} {
         allow read, write: if request.auth != null && request.auth.uid == userId;
       }
     }
   }
   ```

4. In Firebase project settings, register a web app and copy its `apiKey`, `authDomain`, `projectId`, and `appId` into `FIREBASE_CONFIG` near the top of the persistence code in `index.html`. These web app settings are public identifiers, not server credentials; the Firestore rules protect the saved data.
5. Commit and deploy the updated `index.html` to GitHub Pages. On the device with existing progress, create an account in the page; on each other device, sign in with the same account.

The page continues to save locally when signed out. When both the browser and account already have different progress, it asks which copy to keep. Firebase's free quotas can change; stay on Spark and within the displayed quotas to avoid charges.