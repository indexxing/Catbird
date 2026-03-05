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

### Missing Package Product Errors
If you see "Missing package product" errors or remote packages (like Nuke, GRDB, etc.) fail to resolve:

1. **Verify Directory Structure**: Ensure the folders are named exactly `Catbird`, `Petrel`, and `CatbirdMLSCore` and are all inside the same parent directory. No extra nested folders (e.g., `Catbird/Catbird/...`).
2. **Reset Package Caches**:
   - In Xcode, go to **File > Packages > Reset Package Caches**.
   - This forces Xcode to redownload all remote dependencies and relink local ones.
3. **Resolve Package Versions**:
   - Go to **File > Packages > Resolve Package Versions**.
4. **Clean Build Folder**:
   - Press `⌘⇧K` (Command-Shift-K) to clean the build folder.
5. **Clear Derived Data**:
   - Go to **Xcode Settings > Locations**.
   - Click the arrow next to the **Derived Data** path.
   - Delete the `Catbird-...` folder in the Finder.
   - Restart Xcode.

### Missing Local Dependencies
If local packages still aren't recognized, check the **File Inspector** for the package in Xcode to ensure the "Location" is correctly pointing to the relative path `../Petrel` or `../CatbirdMLSCore`.

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
