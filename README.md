# Smart Money Tracker V4

## What's in this ZIP
- `www/`: the complete app (S logo / Smart Money branding from the supplied Android Starter, with V3 password-protected features).
- `android/` is generated automatically by Capacitor during the GitHub Actions build.
- `.github/workflows/build-android.yml`: browser-only cloud build workflow.
- The app's HTML, CSS, JavaScript and assets are bundled into the APK. The app interface and saved data do not depend on GitHub Pages.

## V4 features
- S logo and minimal black-and-white Smart Money interface.
- Password setup and unlock; AES-GCM encrypted local transaction data; PBKDF2-SHA256 (310,000 iterations).
- Change password requires current password, new password twice, and final confirmation.
- Income/expense records, categories, notes, edit/delete, search, type/date filters and sorting.
- Encrypted JSON backup export/import.
- Repeat Date and Advance Date. Advance Date uses the selected mode date as the starting point, then advances exactly one day after each saved transaction.
- Transaction entry form stays hidden until Add Transaction is pressed.
- Multiple display currencies and live conversion. Original amount/currency are retained per transaction; changing display currency never overwrites them.
- Exchange rates fetched from the public open.er-api.com endpoint when app is opened online and refreshed periodically while open. Cached rates are used offline. No refresh happens while the app is fully closed.
- The first app session displays a reminder about internet access and rate updates.

## Important V3 migration note
**Before replacing or moving away from V3, open V3 and use Export backup. Keep that encrypted JSON file safe.** V3 did not save an original currency separately for every transaction, so V4 assigns each old V3 transaction the single currency selected in V3 at the time you export/migrate. V4 cannot recover historical per-transaction currencies that V3 never stored.

If V3 and V4 run on the same website origin, V4 can upgrade the existing V3 encrypted local data automatically when you unlock it. A standalone Android APK has its own app storage, so it will not automatically see website data:
1. Export an encrypted backup from V3 first.
2. Install and open V4. Create a temporary V4 password to enter the app.
3. Choose **Import backup** and select the V3 encrypted JSON backup.
4. V4 locks and asks you to unlock. Use the password that was active when you exported the V3 backup.
5. After confirming your transactions are present, you can change the password in Settings if you want.

Do not delete V3 or clear its browser storage until you have checked that the imported V4 data is correct.

## Make a standalone Android APK without Android Studio
1. Create or open a GitHub repository you control. A new repository is simplest. You can use the repository you already have, but download a copy of its current files first.
2. Extract this ZIP on your PC.
3. Upload the **contents inside** the `smart-money-tracker-v4` folder to the root of your GitHub repository. Make sure the `.github/workflows/build-android.yml` file is included. On GitHub's upload page, hidden folders can be easy to miss; check that `.github/workflows/build-android.yml` exists in the repository.
4. Commit the files to the `main` branch.
5. Open the repository on GitHub → **Actions**. If prompted, enable workflows.
6. Select **Build standalone Android APK** → **Run workflow** → **Run workflow**.
7. Wait for the green check mark. Open that workflow run, scroll to **Artifacts**, and download `smart-money-tracker-v4-debug-apk`.
8. Extract the downloaded artifact ZIP. Install `app-debug.apk` on your Android phone. If Android asks, allow installation from that browser/file manager.
9. Open the app. It works offline for tracking and uses internet only to refresh exchange rates.

## What "standalone" means here
This is a native Android APK containing the app's web files inside the APK through Capacitor's Android WebView runtime. It is not a TWA and does not open your GitHub Pages website. The app can open and use its local tracker offline. Internet is only needed for live currency rates.

## Updates and signing
This workflow builds a debug APK for personal testing. Debug builds are signed for installation, but they are not suitable for Play Store publishing. If you later want to publish or distribute updates, set up a permanent release keystore and keep it safe; future updates must use the same signing key. Do not uninstall an app containing data unless you have exported a backup first.

## Data and password warning
- Data is encrypted and stored locally on this device; it does not sync.
- Uninstalling the app or clearing its storage can erase data. Export encrypted backups regularly.
- There is no password reset. If you forget the password, the encrypted data cannot be recovered.
- The password protects the app's stored data but cannot protect a device from someone who controls the device itself.

## Exchange rate note
The free public rate endpoint may be unavailable or rate-limited. If a currency/rate is missing, the app displays “Rate unavailable” instead of inventing a conversion. Last successful rates are cached on the device. Rates refresh on launch, when returning to the open app, when the user presses Refresh, and every six hours while the app remains open and visible. Nothing updates while the app is fully closed.
