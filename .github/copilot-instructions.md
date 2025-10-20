# SSH_Locker AI Development Guide

This guide provides essential context for AI agents working with the SSH_Locker project.

## Project Overview

SSH_Locker is a macOS utility that automatically stops the SSH agent when the keychain is locked. It integrates with macOS's Security framework to monitor keychain events and manage the SSH agent lifecycle.

## Key Components

### Main Application (`ssh_locker.m`)
- Core functionality that monitors keychain events using Security framework callbacks
- Implements SSH agent management through launchctl commands
- Uses Foundation framework for Objective-C runtime and process management

### Launch Agent (`org.zetam.yves.ssh_locker.plist`)
- System launch daemon configuration
- Ensures SSH_Locker runs automatically at system startup
- Installation path: `/Library/Application Support/SSH_locker/agent/ssh_locker`

## Development Environment

- **Platform**: macOS (10.5+)
- **Build System**: Xcode project
- **Frameworks**:
  - Foundation.framework
  - Security.framework

## Build Process

The project uses Xcode's build system with the following configurations:
- Debug: Development build with debugging symbols
- Release: Optimized build for ppc/i386 architectures

## Development Patterns

1. **Keychain Event Handling**
   - Uses `SecKeychainAddCallback` to register for lock events
   - Event masks defined in Security framework (`kSecLockEventMask`)

2. **Process Management**
   - Uses `NSTask` for launching system commands
   - Synchronous execution with `waitUntilExit`

3. **Memory Management**
   - Uses autorelease pool for Objective-C object lifecycle
   - Manual pool drain in main event loop

## File Organization
```
ssh_locker/
├── ssh_locker.m          # Main application source
├── ssh_locker.1         # Man page documentation
├── ssh_locker_Prefix.pch # Precompiled header
└── org.zetam.yves.ssh_locker.plist # Launch agent configuration
```

## Common Tasks

1. **Building the Project**
   ```bash
   xcodebuild -configuration Release
   ```

2. **Installation**
   - Binary should be installed to `/Library/Application Support/SSH_locker/agent/`
   - Launch agent plist should be installed to `/Library/LaunchAgents/`

## Testing Guidelines

- Verify agent termination when keychain is locked
- Test automatic startup via launch agent
- Ensure clean shutdown and resource cleanup