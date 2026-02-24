# Local Xcode Setup Guide

This guide provides instructions for setting up the Catbird project locally for development in Xcode.

## Prerequisites

- **Xcode 16.0+**: Required for Swift 6 features and iOS 18+ support.
- **Swift 6.0+**: The project uses modern Swift concurrency and `@Observable` macros.
- **iOS 18.0+**: Minimum deployment target for most features.
- **macOS 13.0+**: Required for macOS desktop support.

## Repository Setup

Catbird relies on several sibling repositories for its core functionality. You must clone these into the same parent directory as the Catbird repository.

### 1. Clone the Repositories

```bash
# Create a development directory
mkdir -p CatbirdProject
cd CatbirdProject

# Clone the main app
git clone https://github.com/joshlacal/Catbird.git

# Clone the required sibling dependencies
git clone https://github.com/joshlacal/Petrel.git
git clone https://github.com/joshlacal/CatbirdMLSCore.git
```

### 2. Sibling Dependency Structure

The Xcode project is configured to look for these dependencies at the same level:
- `../Petrel`
- `../CatbirdMLSCore`

## Building and Running

1. Open `Catbird.xcodeproj` in Xcode.
2. Wait for Swift Package dependencies (Nuke, SQLCipher, Sentry, etc.) to resolve.
3. Select the **Catbird** scheme.
4. Choose an iOS Simulator (e.g., iPhone 16 Pro) or a connected device.
5. Press `⌘R` to build and run.

## Project Services

Catbird integrates with several services hosted at `catbird.blue`:
- **Gateway (BFF)**: `https://api.catbird.blue` - Handles OAuth and proxies AT Protocol requests.
- **Notifications**: `https://notifications.catbird.blue` - Handles push notification registration and delivery.

## Troubleshooting

### Missing Dependencies
If you see errors about missing local packages, ensure that `Petrel` and `CatbirdMLSCore` are in the correct sibling directories relative to the `Catbird` folder.

### Signing & Capabilities
The app uses several entitlements:
- **App Groups**: `group.blue.catbird.shared` is used for sharing data between the main app and its extensions (widgets, notification service).
- **Keychain Sharing**: Used for secure credential storage across the app family.

You may need to update the Bundle Identifier and App Group if you are building for a physical device with your own developer account.

## Testing

The project uses the **Swift Testing** framework.
- Open the Test Navigator (`⌘6`) in Xcode.
- Run individual tests or the entire suite using the play buttons next to the test names.
- Ensure that you are testing on an iOS 18+ simulator for full compatibility.
