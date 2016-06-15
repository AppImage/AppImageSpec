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

## Specification

### Image format

The image format determines how an AppImage is represented on disk. Currently there is only one defined image format, however, there could be additional image formats in the future.

#### Type 1

An AppImage which conforms to the type 1 image format:
* **MUST** be an [ISO 9660](http://www.ecma-international.org/publications/standards/Ecma-119.htm) file
* **MUST** use [Rock Ridge](http://www.ymi.com/ymi/sites/default/files/pdf/Rockridge.pdf) extensions
* **MUST** be a vaild [ELF](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format) executable 
* **MUST**, when executed, mount the AppImage and execute the executable file `AppRun` contained in the root of the ISO 9660 filesystem
* **MUST NOT** rely on any specific file name extension, although it is **RECOMMENDED** that the file name extension `.AppImage` is used whenever a file name extension is desired. Futher it is **RECOMMENDED** to follow the naming scheme `ApplicationName-$VERSION-$ARCH.AppImage` in cases in which it is desired to convey this information in the file name
* **SHOULD** not be encapsulated in another archive/container format during download or when stored
* **MUST** work even when stored in a filesystem path that contains blanks or when stored with a file name that contains blanks
* **MAY** embed [update information](#updateinformation) in the ISO 9660 Volume Descriptor field (offset 33651).

### Metadata

#### <a name="updateinformation"></a>Update information

An AppImage **MAY** have update information embedded. Currently two transport mechanisms are available:
 * zsync
 * bintray-zsync
 
##### zsync

The __zsync__ transport requires a HTTP server that can handle HTTP range requests. Its update information is in the form

```
zsync|http://server.domain/path/Application-latest-x86_64.AppImage.zsync
```

For an overview about zsync and how to create `.zsync` files, see [http://zsync.moria.org.uk/](http://zsync.moria.org.uk/). See [http://zsync.moria.org.uk/server-issues](http://zsync.moria.org.uk/server-issues) for more information.

##### bintray-zsync

The __bintray-zsync__ transport extends the zsync transport in that it uses version information from [Bintray](http://bintray.com/). Its update information is in the form

```
bintray-zsync|probono|AppImages|Subsurface|Subsurface-_latestVersion-x86_64.AppImage.zsync
```
