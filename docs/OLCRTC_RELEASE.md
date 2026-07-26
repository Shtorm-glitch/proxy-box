# Proxy-box release

## Scope

Version `1.13.14-olcrtc.2` is a personal arm64 Android release of the SFA user interface with an OLCRTC-enabled sing-box-extended core.

The rotation implementation uses two ordered rooms, make-before-break carriers, active/prepared/draining states, per-carrier recovery, and epoch plus room-session identifiers. Existing TCP connections are not migrated. New connections use the active carrier.

The server operates autonomously. NAS is only the room owner and does not send room status to the server. After owner loss, the server waits 5 seconds before trying the next room. A successful server carrier in that room triggers transition of the old room to DRAINING within 10 seconds. Existing connections remain supported for a randomized 12 to 17 minute drain timeout. If the next room is unavailable, the old room remains active for new and existing connections and retries occur after randomized delays from 2 to 7 minutes.

## Android configuration safety

The first route rule must reject ICMP. This is an accepted workaround for an upstream sing-tun ICMP echo proxy mapping leak that can create tens of thousands of file descriptors and excessive RSS when echo responses are absent.

Do not enable IPv6 or `strict_route` on client networks without IPv6. The public repository intentionally contains no room URLs, credentials, tokens, server addresses, signing keys, or complete user profiles.

## Build lineage

This APK uses the following corresponding sources:

- Android UI: this repository, tag `v1.13.14-olcrtc.2`.
- Core: `Shtorm-glitch/sing-box-extended`, tag `v1.13.11-extended-2.1.0-olcrtc.1`.
- OLCRTC: `Shtorm-glitch/olcrtc`, tag `v0.0.1-olcrtc.1`.
- LiveKit SDK: `Shtorm-glitch/server-sdk-go`, tag `v2.16.4-olcrtc.1`.

The direct bases are `SagerNet/sing-box-for-android`, `shtorm-7/sing-box-extended`, `SagerNet/sing-box`, `openlibrecommunity/olcrtc`, and `livekit/server-sdk-go`.

## Reproducible build outline

1. Check out the tagged sing-box-extended, OLCRTC, and LiveKit forks listed above.
2. Install Go 1.26+, Android SDK/NDK, gomobile, and JDK 17 or 21.
3. From sing-box-extended, run `go run ./cmd/internal/build_libbox -target android -platform android/arm64` to create and copy `libbox.aar` into `app/libs` of a sibling Android checkout.
4. Check out this Android tag and provide release signing properties through base64-encoded `LOCAL_PROPERTIES`. Keep the keystore outside the repository.
5. Run `gradlew.bat :app:assembleOtherRelease` on Windows or `./gradlew :app:assembleOtherRelease` on Unix.

Reproduction with a different private signing key produces a cryptographically different APK. Verify source version, manifest, ABI, and native libraries in addition to the signature.

## Release verification

- Android package: `io.nekohasekai.sfa`.
- Version code: `687`.
- Version name: `1.13.14-olcrtc.2`.
- ABI: arm64-v8a only.
- Build type: release, R8/minification enabled, not debuggable.
- Expected signing certificate SHA-256: `C8:F8:40:52:25:A2:0B:92:E5:03:61:B4:C8:90:FA:01:6F:1C:49:83:55:C0:E8:49:D1:0C:65:D9:BF:54:88:9D`.

## Validation performed

- Targeted OLCRTC tests passed in the library and sing-box integration.
- Release assembly with R8 completed.
- APK v1 and v2 signatures verified.
- Manifest is not debuggable and contains only arm64-v8a native libraries.
- No private profile, room configuration, credential, or signing material is packaged.
- Installation, profile import, VPN start, YouTube traffic, and VPN stop/start smoke tests passed on Android.

The inherited Android fork currently reports pre-existing lint failures in Compose resource access, translations, and privileged/Xposed support. No lint error points to OLCRTC integration. This remains a known limitation for a future public-store-quality build.
