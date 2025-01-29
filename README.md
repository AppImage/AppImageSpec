# AppImage Specification README

----

*For direct access to the current specification in writing please go to [current.md](https://github.com/AppImage/AppImageSpec/blob/master/current.md).*

----

This repository holds the specification for the AppImage format. The objectives of this specification are to:

1. Be a single, definitive source of information about the AppImage format
2. Ensure AppImages behave in a consistent manner, thereby enabling third-party developers to write AppImage management applications and distributions to offer built-in support for AppImages
3. Serve as a guide for [appimagetool](https://github.com/probonopd/appimagetool) development
4. Provide developers with a checklist for creating AppImages
5. Enable a lintian-style application to be written to check AppImages for compliance with the standard ([start here](https://github.com/probonopd/AppImages/blob/master/appdir-lint.sh))
6. Indicate the direction of the project and warn developers of future changes to the AppImage format
7. Document the open nature of the AppImage format

[appimagetool](https://github.com/probonopd/appimagetool) is becoming a reference implementation of building standards-compliant AppImages, and the some AppImages from [pkg2appimage](https://github.com/AppImageCommunity/pkg2appimage) repository provides examples.

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
[current.md](current.md) | Working document with proposed changes for the next specification release
