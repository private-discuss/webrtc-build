# Private Discuss build of LiveKitWebRTC

Fork of [webrtc-sdk/webrtc-build](https://github.com/webrtc-sdk/webrtc-build) at tag
`m144.7559.15` (the build behind `@livekit/react-native-webrtc` 144.x) with one change:
`build/patches/raw_frame_key.patch` — the FrameCryptor encrypts / decrypts media frames with the
raw AES key material instead of a PBKDF2-derived AES-128 key, so mobile interoperates with the
Private Discuss web client and native apps (raw AES-256-GCM frame key, RSA-OAEP-256 wrapped).

The workflow builds only `apple_prefixed` (→ `LiveKitWebRTC.xcframework.zip`) and
`android_prefixed` (→ `webrtc.tar.gz` + `android-prefixed-<tag>.aar`) on GitHub-hosted runners
and attaches them to a GitHub release when a tag is pushed.

## Release

```
git tag m144.7559.15-pd.1
git push origin m144.7559.15-pd.1
```

Then wait for the `build` workflow (iOS ≈ 2–3 h on `macos-26`, Android ≈ 3–4 h on
`ubuntu-latest`) and copy the asset URLs + `SHA256SUMS.txt` into the app's
`docs/native/webrtc-build.md`.

The consumer side (local `LiveKitWebRTC` podspec, Gradle substitution of
`io.github.webrtc-sdk:android-prefixed`) lives in the `private-discuss-react-native` repo.
