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
Reference implementation for building AppImages. https://github.com/probonopd/AppImageKit

##### <a name="desktop file"></a>.desktop file
A file following the [Desktop Entry Specification](https://specifications.freedesktop.org/desktop-entry-spec/1.1/).

##### <a name="mimeTypes"></a>Mime Types
Mime type definitions are spread across several resources. The mime type definitions should be in compliance with [RFC 6838](http://tools.ietf.org/html/rfc6838)

##### <a name="payloadap"></a>Payload application
The application software contained inside an AppImage

##### <a name="Target system"></a>Target system(s)
The target system(s) the application software packaged as an AppImage is intended to run on. The person or group creating an AppImage can decide which target systems the AppImage needs to run on, and **SHOULD** communicate the system requirements clearly to users

##### <a name="zsync"></a>zsync
A file transfer algorithm which implements efficient download of only the content of a file which is not already known to the receiver. http://zsync.moria.org.uk/paper/

## Specification

### Image format

The image format determines how an AppImage is represented on disk. Currently there is only one defined image format, however, there could be additional image formats in the future.

#### Type 0 image format

Reserved for not fully standards-compliant AppImages (e.g., portable binaries that look and behave like AppImages on a user level, but do not conform to a standard on a programming level). 

#### Type 1 image format

An AppImage which conforms to the type 1 image format:

* **MUST** be an [ISO 9660](http://www.ecma-international.org/publications/standards/Ecma-119.htm) file
* **MUST** use [Rock Ridge](http://www.ymi.com/ymi/sites/default/files/pdf/Rockridge.pdf) extensions
* **MAY** use [Joliet](http://support.microsoft.com/kb/125630) extensions
* **SHOULD** use [zisofs](http://libburnia-project.org/wiki/zisofs) compression
* **MUST** be a vaild [ELF](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format) executable 
* **MUST**, when executed, mount the AppImage and execute the executable file `AppRun` contained in the root of the ISO 9660 filesystem
* **MUST NOT** rely on any specific file name extension, although it is **RECOMMENDED** that the file name extension `.AppImage` is used whenever a file name extension is desired. Futher it is **RECOMMENDED** to follow the naming scheme `ApplicationName-$VERSION-$ARCH.AppImage` in cases in which it is desired to convey this information in the file name
* **SHOULD** not be encapsulated in another archive/container format during download or when stored
* **MUST** work even when stored in a filesystem path that contains blanks or when stored with a file name that contains blanks
* **MAY** embed [update information](#updateinformation) in the ISO 9660 Volume Descriptor field (offset 33651). If the information in this location is not in one of the known [update information](#updateinformation) formats, then it **SHOULD** be empty and/or be ignored.

### Contents of the image

The filesystem image:
* Is an AppDir
* **MUST** contain a file named `AppRun` in its root directory
* **SHOULD** contain a payload application that is ultimately executed when the AppImage is executed
* **SHOULD** contain exactly one `$APPNAME.desktop` file in its root directory with `$APPNAME` being the name of the payload application
* **MAY** contain an `$APPNAME.png` file in its root directory with `$APPNAME` being the name of the payload application as set in the `Icon=` key of the `$APPNAME.desktop` file. If present, this icon **SHOULD** be given preference as the icon being used to represent the AppImage. The icon **SHOULD** be a png with 256x256 or 512*512 pixels
* **SHOULD** contain a `.DirIcon` file as per the AppDir specification

The `AppRun` file:
* **MUST** be executable and, when executed, launch the payload application either directly or indirectly (i.e., by invoking other helper binaries or scripts which, in turn, launch the payload application)
* **MAY** be an ELF binary or an interpreted script
* If it is an ELF binary, it **SHOULD** have as few runtime dependencies as possible
* If it is an interpreted script, it **SHOULD** be written in a language in which an interpreter can be assumed to be available on every target system
* **MUST** work even when stored in a filesystem path that contains blanks or when stored with a file name that contains blanks
* **SHOULD** pass any arguments passed to it to the payload application either directly or indirectly (i.e., by invoking other helper binaries or scripts which, in turn, launch the payload application) if it is not explicitly deemed useful otherwise
* **SHOULD** pass any environment variables passed to it to the payload application either directly or indirectly (i.e., by invoking other helper binaries or scripts which, in turn, launch the payload application) if it is not explicitly deemed useful otherwise
* **MAY** `cd` to a directory inside the AppImage at runtime before executing the payload application, commonly `./usr/` 

The payload application:
* **MAY** be an ELF binary or an interpreted script
* If it is an ELF binary, it **SHOULD** have as few dynamic library dependencies as possible and each dynamic library dependency **MUST** be included in the AppImage *IF* it cannot be assumed to be part of every target system in a recent enough version
* If it is an interpreted script, it **SHOULD** be written in a language in which an interpreter can be assumed to be available on every target system, otherwise the interpreter **MUST** be included in the AppImage
* It is **RECOMMENDED** that the payload application and its dependencies are located in a `$PREFIX` directory tree inside the AppDir with `$PREFIX` commonly being `./usr/`; it is **RECOMMENDED** that the `$PREFIX` directory tree inside the AppDir follows the [File System Hierarchy conventions for `/usr`](http://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch04.html)

### Metadata

Setion to be expanded; additional metadata is being discussed on https://github.com/probonopd/AppImageKit/issues/59

#### <a name="updateinformation"></a>Update information

An AppImage **MAY** have update information embedded for exactly one transport mechanism. The location in which this information is stored is defined by the [image format](#image-format). Currently two transport mechanisms are available, but only one can be used for each given AppImage:
 * zsync
 * bintray-zsync
 
##### zsync

The __zsync__ transport requires a HTTP server that can handle HTTP range requests. Its update information is in the form

```
zsync|http://server.domain/path/Application-latest-x86_64.AppImage.zsync
```

If an AppImage has update information embedded for this transport mechanism, then the following fields **MUST** be used; separated by a "|" character:

Field | Type | Example | Comments
----------- | ------ | -------- | --------
Transport mechanism | String | `zsync` | zsync file and AppImage **MUST** be stored on  [compatible](http://zsync.moria.org.uk/server-issues) HTTP server
zsync file URL | String | `http://server.domain/path/Application-latest-x86_64.AppImage.zsync` | URL to the `.zsync` file (URL **MUST NOT** change from version to version)

For an overview about zsync and how to create `.zsync` files, see [http://zsync.moria.org.uk/](http://zsync.moria.org.uk/).

##### bintray-zsync

The __bintray-zsync__ transport extends the zsync transport in that it uses version information from [Bintray](http://bintray.com/). Its update information is in the form

```
bintray-zsync|probono|AppImages|Subsurface|Subsurface-_latestVersion-x86_64.AppImage.zsync
```

If an AppImage has update information embedded for this transport mechanism, then the following fields **MUST** be used; separated by a "|" character:

Field | Type | Example | Comments
----------- | ------ | -------- | --------
Transport mechanism | String | `bintray-zsync` | zsync file and AppImage **MUST** be stored on Bintray
Bintray username | String | `probono` | Name of the user or organization of the account where the zsync file and AppImage are stored
Bintray repository | String | `AppImages` | Name of the repository in which the zsync file and AppImage are stored
Bintray package name | String | `Subsurface` | Name of the [Bintray package](https://bintray.com/docs/usermanual/uploads/uploads_creatinganewpackage.html) in which the zsync file and AppImage are stored
Bintray zsync path | String | `Subsurface-_latestVersion-x86_64.AppImage.zsync` | Path where the zsync file is stored on Bintray (**MUST NOT** change from version to version). Note the use of the Bintray feature `_latestVersion` to facilitate this

### Desktop integration

The software inside the AppImage **MAY** integrate into the desktop environment (e.g., by installing a `.desktop` file into the host system) on the user's behalf. However if it does so, it *SHOULD* ensure to get the explicit permission of the user, e.g., by asking for permission in a GUI window, or by reacting to a command line argument.

The software inside the AppImage **SHOULD NOT** attempt to do desktop integration if at least one of the following conditions are met:
* A file `$HOME/.local/share/appimagekit/no_desktopintegration` exists on the target system
* A file `/usr/share/appimagekit/no_desktopintegration` exists on the target system
* A file `/etc/appimagekit/no_desktopintegration` exists on the target system
* A process named `appimaged` is running on the system
* The environment variable `$DESKTOPINTEGRATION` is not empty
