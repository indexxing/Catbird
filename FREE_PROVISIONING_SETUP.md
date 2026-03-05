# Setup Guide: Free Apple Developer Account

This guide provides instructions for setting up and running Catbird using a free Apple ID (Personal Team). Since a free account does not support certain capabilities (like Push Notifications and specific App Group configurations), you will need to make some adjustments to the project.

## ⚠️ Important Note
These changes are for **local development only**. Do not commit these changes to the repository if you plan on contributing back to the project, as they will break the production build and signing.

## Prerequisites
- **Xcode 16.0+**
- **A physical iOS device** (optional, for testing on hardware) or the **iOS Simulator**.

## 1. Change the Bundle Identifier
Free accounts require a unique Bundle Identifier that hasn't been used before.

1. Open the project in Xcode.
2. Select the **Catbird** project in the Project Navigator.
3. Select the **Catbird** target.
4. Go to the **Signing & Capabilities** tab.
5. In the **Bundle Identifier** field, change `blue.catbird` to something unique (e.g., `com.yourname.catbird`).
6. Repeat this for the extension targets:
   - **CatbirdNotificationWidgetExtension**
   - **CatbirdFeedWidgetExtension**
   - **NotificationServiceExtension**
   - **SharedDraftImporter**

## 2. Select Your Personal Team
1. In the **Signing & Capabilities** tab for each target:
2. Change the **Team** dropdown to your own Name (Personal Team).

## 3. Handle Unsupported Capabilities

### Push Notifications
Free accounts do not support the "Push Notifications" capability on physical devices.

1. In the **Signing & Capabilities** tab for the **Catbird** target:
2. Remove the **Push Notifications** capability by clicking the "x" next to it.
3. The app will still run, but remote notifications will not be received.

### App Groups
Free accounts have limitations with App Groups.

1. In the **Signing & Capabilities** tab for **all targets**:
2. Find the **App Groups** section.
3. Remove the existing `group.blue.catbird.shared`.
4. Add a new App Group with a unique name (e.g., `group.com.yourname.catbird.shared`).
5. **CRITICAL**: You must also update this identifier in the code. Search for `group.blue.catbird.shared` in the entire project and replace it with your new group ID.

### Keychain Sharing
1. In the **Signing & Capabilities** tab for the **Catbird** target:
2. Remove the **Keychain Sharing** capability.
3. The app uses the keychain for secure storage. Removing this may limit sharing credentials between the app and its extensions, but the main app will still function.

## 4. Running on Device
If you are running on a physical device:
1. After the first install, you will see an "Untrusted Developer" message.
2. Go to **Settings > General > VPN & Device Management** on your iPhone.
3. Tap your Apple ID and select **Trust**.

## 5. Simulator Testing
Most of these limitations do **not** apply to the iOS Simulator. If you do not have a paid license, testing in the **Simulator** is highly recommended as it allows you to test most features without modifying the signing configuration extensively.

---

## Troubleshooting

### "Communication with Apple failed"
This often happens if the Bundle Identifier is already taken. Try choosing a more unique name.

### "Provisioning profile doesn't support..."
This means you haven't removed all the paid-only capabilities. Ensure Push Notifications and iCloud (if any) are removed from the Signing & Capabilities tab.
