# MagicQuit (Fork)

A macOS menu bar app that automatically quits idle applications after a configurable period.

This is a fork of [BigBerny/magicquit](https://github.com/BigBerny/magicquit) with the following modifications:

- **Opt-in model**: Apps are no longer killed by default. Only explicitly selected apps are terminated after the idle period.
- **Granular time controls**: Idle time can be set from 15 minutes up to 72 hours (previously 1-72 hours only).
- **Scaled urgency indicator**: App names go bold when less than 25% of the configured time remains, instead of using a fixed 1-hour threshold.

## Building

```bash
# Using Xcode CLI
xcodebuild -scheme MagicQuit -configuration Release build

# Or open in Xcode
open MagicQuit.xcodeproj
```

Requires macOS 13.3+ and Xcode with Swift 5.

## License

This project is licensed under the [GNU General Public License v3.0](LICENSE), the same license as the original project.
