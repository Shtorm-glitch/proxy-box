# OLCRTC Android

Unofficial Android client with OLCRTC carrier rotation, based on SFA.

This is not an official SagerNet, sing-box, sing-box-extended, OLCRTC, or LiveKit release. The application is published under the separate name **OLCRTC Android** to avoid implying association with the upstream SFA application.

## Project relationship

- [SagerNet/sing-box-for-android](https://github.com/SagerNet/sing-box-for-android) is the original Android application and direct UI base.
- [SagerNet/sing-box](https://github.com/SagerNet/sing-box) is the original sing-box project.
- [shtorm-7/sing-box-extended](https://github.com/shtorm-7/sing-box-extended) is the extended sing-box fork used as the direct core base.
- [Shtorm-glitch/sing-box-extended](https://github.com/Shtorm-glitch/sing-box-extended), tag `v1.13.11-extended-2.1.0-olcrtc.1`, contains the OLCRTC endpoint, outbound, and autonomous rotation logic packaged by this app.
- [openlibrecommunity/olcrtc](https://github.com/openlibrecommunity/olcrtc) is the original OLCRTC project.
- [Shtorm-glitch/olcrtc](https://github.com/Shtorm-glitch/olcrtc), tag `v0.0.1-olcrtc.1`, contains the OLCRTC carrier extensions used by the core.
- [Shtorm-glitch/server-sdk-go](https://github.com/Shtorm-glitch/server-sdk-go), tag `v2.16.4-olcrtc.1`, contains the minimal LiveKit SDK additions required by rotation.

See [OLCRTC release documentation](docs/OLCRTC_RELEASE.md) for architecture, configuration safety, build instructions, verification, and known limitations.

## License and attribution

The Android application remains licensed under GPLv3 with the additional upstream naming and association condition in [LICENSE](LICENSE). See [NOTICE.md](NOTICE.md) for source lineage and attribution.

Copyright for upstream components remains with their respective authors and contributors.
