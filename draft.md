# AppImage Specification

#### Working Draft

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](http://www.ietf.org/rfc/rfc2119.txt).

The AppImage Specification is licensed under [The MIT License](https://github.com/AppImage/Spec/blob/master/LICENSE).

## Introduction

The AppImage Specification describes AppImage, a format to deploy application software to Linux-based operating systems.

## Revision History

Version | Date | Notes
--- | --- | ---
Draft | 2016-06-15 | Initial draft of the AppImage Specification started

## Definitions

##### <a name="AppDir"></a>AppDir
Application directories as used in the ROX desktop. http://rox.sourceforge.net/desktop/AppDirs.html

##### <a name="AppImage"></a>AppImage
The AppImage file format which can be used to deploy application software to Linux-based operating systems. Depending on the context, the term can also refer to an application packaged in this format.

##### <a name="AppImageKit"></a>AppImageKit
F reference implementation for building AppImages. https://github.com/probonopd/AppImageKit

##### <a name="Base system"></a>Base system(s)
The target system(s) the application software packaged as an AppImage is intended to run on.

##### <a name="desktop file"></a>.desktop file
A file following the [Desktop Entry Specification](https://specifications.freedesktop.org/desktop-entry-spec/1.1/).

##### <a name="mimeTypes"></a>Mime Types
Mime type definitions are spread across several resources. The mime type definitions should be in compliance with [RFC 6838](http://tools.ietf.org/html/rfc6838).

##### <a name="zsync"></a>zsync
A file transfer algorithm which implements efficient download of only the content of a file which is not already known to the receiver. http://zsync.moria.org.uk/paper/
