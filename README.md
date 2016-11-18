# iCepa

![Travis CI](https://img.shields.io/travis/iCepa/iCepa.s
vg)

iCepa is an iOS system-wide VPN Tor client. It uses [`Tor.framework`](https://github.com/iCepa/Tor.framework) to manage its Tor instance, and [`tun2tor`](https://github.com/iCepa/tun2tor) to bridge VPN traffic to Tor. The project does *not* work yet, and is in progress.

## Requirements

- iOS 10 or later
- Xcode 8 or later

## Building

1. Because the network extension depends on [`tun2tor`](https://github.com/iCepa/tun2tor), you will need Rust installed. You can install it using [rustup](https://www.rustup.rs):

    ```sh
curl https://sh.rustup.rs -sSf | sh
rustup install stable
rustup target add aarch64-apple-ios
rustup target add armv7s-apple-ios
rustup target add armv7-apple-ios
```

2. iCepa also depends on [`Tor.framework`](https://github.com/iCepa/Tor.framework), which you can build using [`Carthage`](https://github.com/Carthage/Carthage):

    ```sh
git submodule update --init --recursive
brew install automake autoconf libtool gettext carthage
carthage build
```

3. iCepa should now build normally from Xcode. If it does not, please [file an issue](https://github.com/iCepa/iCepa/issues/new)!

## Installation

Installing this application on your own iOS device requires some manual set up in Apple's [developer portal](https://developer.apple.com/account/ios/identifier/bundle):

1. Pick a unique bundle identifier (`com.foo.iCepa`) and generate an `App ID` for it.
2. Append a new component to that bundle identifier to form the extension's bundle identifier (`com.foo.iCepa.extension`), and generate an `App ID` for that new bundle identifier.
3. Create an `App Group` (`group.com.foo.iCepa`), and enable that `App Group` on both of the `App ID`s that you just created.
4. Create two new development `Provisioning Profile`s, one for each `App ID`, and enable the `Network Extension iOS (Dev)` entitlements on both.
5. Place the `App Group` and both `App ID`s in `iCepa-iOS.xcconfig`

## Contributing

iCepa is separated into a two components:

- The UI is written in Swift, and provides a basic interface to start, stop and configure the Tor network extension.
- The network extension itself is also written in Swift, and bridges traffic to Tor using an `NEPacketTunnelProvider` and [`tun2tor`](https://github.com/iCepa/tun2tor). An `NEPacketTunnelProvider` is analogous to a `utun` (userspace network tunnel) interface.

[`Tor.framework`](https://github.com/iCepa/Tor.framework) is used to communicate with and start the `tor` instance from both the app and the extension.

Things that need work:
- `tun2tor` parses packets and handles DNS traffic correctly, but does not forward TCP streams over SOCKS to tor. Reach out to [@conradev](https://github.com/conradev) if you are interested in helping with `tun2tor`.
- The UI needs work. The main focus is the control screen which will have controls to start/stop and information about the connection. Taking mockups/pull requests for either! Create Github issues for now.
- There is no icon or any branding (the name is not even final).
