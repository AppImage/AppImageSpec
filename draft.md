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

##### <a name="Base system"></a>Base system(s)
The target system(s) the application software packaged as an AppImage is intended to run on.

##### <a name="mimeTypes"></a>Mime Types
Mime type definitions are spread across several resources. The mime type definitions should be in compliance with [RFC 6838](http://tools.ietf.org/html/rfc6838).
