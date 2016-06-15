# AppImage Specification

This repository holds the specification for the AppImage format Its objectives are to:

1. Be a single, definitive source of information about the AppImage format
2. Ensure AppImages behave in a consistent manner, thereby enabling third-party developers to write AppImage management applications and distributions to offer built-in support for AppImages
3. Serve as a guide for AppImageKit development
4. Provide a developer who with a checklist for creating AppImages
5. Enable a lintian-style application to be written to check AppImages for compliance with the standard
6. Indicate the direction of the project and warn developers of future changes to the AppImage format
7. Document the open nature of the AppImage format

[AppImageKit](https://github.com/probonopd/AppImageKit) is becoming a reference implementation of building standards-compliant AppImages, and the [AppImages](https://github.com/probonopd/AppImages) repository provides examples.

## Contributing

* Proposals are discussed via GitHub Issues
* Pull request can be sent for final approval by the maintainer, currently @probonopd
* The specification is to be kept as concise as possible and will not include any kind of explanation, justification or help with implementation (although it may link to that information)
* The specification itself will only include the "what", and not the "why" or "how"
* Detailed justifications and explanation can be preserved by linking to the discussion in the actual pull request
* General help on creating AppImages is not part of this specification but can be found in the [AppImageKit wiki](https://github.com/probonopd/AppImageKit/wiki) 

## Organization of this repository

File | Purpose
---|---
[README.md](README.md) | Instructions for contributing
[LICENSE](LICENSE) | Copyright and license
[draft.md](draft.md) | Working document with proposed changes for the next specification release
versions/ | Directory for released versions of the specification
versions/v1.0.md | Version 1.0 of the specification
versions/v2.0.md | ...
