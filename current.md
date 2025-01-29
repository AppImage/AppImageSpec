# AppImage Specification

#### Working Draft

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

The AppImage Specification is licensed under [The MIT License](https://github.com/AppImage/Spec/blob/master/LICENSE).

##### Table of Contents

* [Introduction]
* [Revision History]
* [Definitions]
* [Specification]
    - [Image format]
        + [Type 0 image format]
        + [Type 1 image format]
        + [Type 2 image format]
    - [Contents of the image]
        + [The filesystem image]
        + [The AppRun file]
        + [The payload application]
    - [Metadata]
        + [AppStream]
        + [Update Information]
    - [Runtime]
    - [Desktop Integration]

[Introduction]: #introduction "Introduction"
[Revision History]: #revision-history "Revision History"
[Definitions]: #definitions "Definitions"
[Specification]: #specification "Specification"
[Image format]: #image-format "Specification - Image Format"
  [Type 0 image format]: #type-0-image-format "Specification - Image Format - Type 0 image format"
  [Type 1 image format]: #type-1-image-format "Specification - Image Format - Type 1 image format"
  [Type 2 image format]: #type-2-image-format "Specification - Image Format - Type 2 image format"
[Contents of the image]: #contents-of-the-image "Specification - Contents of the image"
  [The filesystem image]: #the-filesystem-image "Specification - Contents of the image - The filesystem image"
  [The AppRun file]: #the-apprun-file "Specification - Contents of the image - The AppRun file"
  [The payload application]: #the-payload-application "Specification - Contents of the image - The payload application"
[Metadata]: #metadata "Specification - Metadata"
  [AppStream]: #appstream "Specification - Metadata - AppStream"
  [Update Information]: #update-information "Specification - Metadata - Update Information"
[Desktop Integration]: #desktop-integration "Specification - Desktop Integration"

## Introduction

The AppImage Specification describes [AppImage], a format to deploy application software to Linux-based operating systems.

## Revision History

Version | Date | Notes
--- | --- | ---
Draft | 2016-06-15 | Initial draft of the AppImage Specification started
Draft | 2016-09-18 | Type 2 image format drafted

## Definitions

##### AppDir
Application directories as used in the [ROX Desktop](http://rox.sourceforge.net/) - <http://rox.sourceforge.net/desktop/AppDirs.html>

[AppDir]: #appdir "AppDir: Application Directory (as used in the ROX Desktop)"

##### AppImage
The AppImage file format which can be used to deploy application software to Linux-based operating systems. Depending on the context, the term can also refer to an application packaged in this format

[AppImage]: #appimage "AppImage: The packaging format defined in this Specification, or an application packaged in this format"

##### AppImageKit
Reference implementation for building AppImages. <https://github.com/probonopd/AppImageKit>

[AppImageKit]: #appimagekit "AppImageKit: A packaging tool for building AppImages"

##### .desktop file
A Desktop Entry File following the [Desktop Entry Specification](https://specifications.freedesktop.org/desktop-entry-spec/latest/)

[.desktop file]: #desktop-file ".desktop file: A Desktop Entry File following the Desktop Entry Specificiation"

##### MIME Types
MIME type definitions are spread across several resources. The MIME type definitions should be in compliance with [RFC 6838](https://tools.ietf.org/html/rfc6838)

[MIME Type]: #mime-type "MIME Type: A media type or file format identifier"

##### Payload application
The software application contained inside an AppImage

[payload application]: #payload-application "Payload application: The software application contained within an AppImage"

##### Target system(s)
The target system(s) the application software packaged as an [AppImage] is intended to run on. The person or group creating an [AppImage] can decide which target systems the [AppImage] needs to run on, and **SHOULD** communicate the system requirements clearly to users

[target system]: #target-systems "Target System(s): The system(s) on which an AppImage is intended to run"

##### zsync
A file transfer algorithm which implements efficient download of only the content of a file which is not already known to the receiver. <http://zsync.moria.org.uk/paper/>

[zsync]: #zsync "zsync: A file transfer program that facilitates delta updates"

## Specification

### Image format

The image format determines how an [AppImage] is represented on disk. Currently there is only one defined image format, however, there could be additional image formats in the future.

#### Type 0 image format

Reserved for not fully standards-compliant AppImages (e.g., portable binaries that look and behave like AppImages on a user level, but do not conform to a standard on a programming level). 

#### Type 1 image format

An [AppImage] which conforms to the type 1 image format:

* **MUST** be an [ISO 9660](https://www.ecma-international.org/publications/standards/Ecma-119.htm) file
* **MUST** use [Rock Ridge](http://www.ymi.com/ymi/sites/default/files/pdf/Rockridge.pdf) extensions
* **MAY** use [Joliet](https://support.microsoft.com/kb/125630) extensions
* **SHOULD** use [zisofs](http://libburnia-project.org/wiki/zisofs) compression
* **MUST** be a valid [ELF](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format) executable 
* **MUST**, when executed, mount the [AppImage] and execute the executable file `AppRun` contained in the root of the ISO 9660 filesystem
* **MUST NOT** rely on any specific file name extension, although it is **RECOMMENDED** that the file name extension `.AppImage` is used whenever a file name extension is desired. Futher it is **RECOMMENDED** to follow the naming scheme `ApplicationName-$VERSION-$ARCH.AppImage` in cases in which it is desired to convey this information in the file name
* **SHOULD** not be encapsulated in another archive/container format during download or when stored
* **MUST** work when spaces are used in its own filesystem path, in its own file name and in paths and filenames it uses internally
* **MAY** embed [update information] in the ISO 9660 Volume Descriptor field (offset 33651). If the information in this location is not in one of the known [update information] formats, then it **SHOULD** be empty and/or be ignored
* **SHOULD** contain the magic hex `0x414901` at offset 8 ([why?](https://github.com/probonopd/AppImageKit/issues/144))

#### Type 2 image format

An [AppImage] which conforms to the type 2 image format:

* **MUST** be a valid [ELF](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format) executable 
* **MUST** have appended to it a filesystem that the ELF part can mount
* **MUST**, when executed, mount the [AppImage] and execute the executable file `AppRun` contained in the root of the filesystem image
* **MUST NOT** rely on any specific file name extension, although it is **RECOMMENDED** that the file name extension `.AppImage` is used whenever a file name extension is desired. Futher it is **RECOMMENDED** to follow the naming scheme `ApplicationName-$VERSION-$ARCH.AppImage` in cases in which it is desired to convey this information in the file name
* **SHOULD** not be encapsulated in another archive/container format during download or when stored
* **MUST** work when spaces are used in its own filesystem path, in its own file name and in paths and filenames it uses internally
* **MAY** embed [update information] in the ELF section `.upd_info`. If the information in this location is not in one of the known [update information] formats, then it **SHOULD** be empty and/or be ignored
* **MAY** embed a digital signature in the ELF section `.sha256_sig`. If this section exists then it **MUST** either be empty (filled with `0x00` padding) or contain a valid digital signature of the sha256 of the AppImage assuming the ELF section `.sha256_sig` being filled with `0x00` padding ([why?](https://github.com/probonopd/AppImageKit/issues/238#issuecomment-249412813))
* **MUST** contain the magic hex `0x414902` at offset 8 ([why?](https://github.com/probonopd/AppImageKit/issues/144))

### Contents of the image

#### The filesystem image:

* Is an [AppDir]
* **MUST** contain a file named `AppRun` in its root directory
* **SHOULD** contain a [payload application] that is ultimately executed when the [AppImage] is executed
* **SHOULD** contain exactly one `$APPNAME.desktop` file in its root directory with `$APPNAME` being the name of the [payload application]
* **SHOULD** contain icon files below `usr/share/icons/hicolor` following the  [Icon Theme Specification](https://standards.freedesktop.org/icon-theme-spec/icon-theme-spec-latest.html) for the icon identifier as set in the `Icon=` key of the `$APPNAME.desktop` file. If present, these icon files **SHOULD** be given preference as the icon being used to represent the [AppImage].
* **MAY** contain an `$APPICON.svg`, `$APPICON.svgz` or `$APPICON.png` file in its root directory with `$APPICON` being the icon identifier as set in the `Icon=` key of the `$APPNAME.desktop` file. If present and no icon files matching the icon identifier present below `usr/share/icons/hicolor`, this icon **SHOULD** be given preference as the icon being used to represent the [AppImage]. If a PNG file, the icon **SHOULD** be of size 256x256, 512x512, or 1024x1024 pixels.
* **MUST** contain a `.DirIcon` file as per the [AppDir] specification which **SHOULD** be a 256x256 PNG file.

#### The `AppRun` file:

* **MUST** be executable and, when executed, launch the [payload application] either directly or indirectly (i.e., by invoking other helper binaries or scripts which, in turn, launch the [payload application])
* **MAY** be an ELF binary or an interpreted script
* If it is an ELF binary, it **SHOULD** have as few runtime dependencies as possible
* If it is an interpreted script, it **SHOULD** be written in a language in which an interpreter can be assumed to be available on every [target system]
* **MUST** work even when stored in a filesystem path that contains blanks
* **SHOULD** pass any arguments passed to it to the [payload application] either directly or indirectly (i.e., by invoking other helper binaries or scripts which, in turn, launch the [payload application]) if it is not explicitly deemed useful otherwise
* **SHOULD** pass any environment variables passed to it to the [payload application] either directly or indirectly (i.e., by invoking other helper binaries or scripts which, in turn, launch the [payload application]) if it is not explicitly deemed useful otherwise
* **MAY** `cd` to a directory inside the [AppImage] at runtime before executing the [payload application], commonly `./usr/` 

#### The [payload application]:

* **MAY** be an ELF binary or an interpreted script
* If it is an ELF binary, it **SHOULD** have as few dynamic library dependencies as possible and each dynamic library dependency **MUST** be included in the [AppImage] *IF* it cannot be assumed to be part of every [target system] in a recent enough version
* If it is an interpreted script, it **SHOULD** be written in a language in which an interpreter can be assumed to be available on every [target system], otherwise the interpreter **MUST** be included in the [AppImage]
* It is **RECOMMENDED** that the [payload application] and its dependencies are located in a `$PREFIX` directory tree inside the [AppDir] with `$PREFIX` commonly being `./usr/`; it is **RECOMMENDED** that the `$PREFIX` directory tree inside the [AppDir] follows the [File System Hierarchy conventions for `/usr`](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch04.html)

### Metadata

Section to be expanded; additional metadata is being discussed on <https://github.com/probonopd/AppImageKit/issues/59>

#### AppStream

An [AppImage] **SHOULD** ship AppStream metadata in `usr/share/metainfo/$ID.appdata.xml` with `$ID` being the AppStream ID. Shipping AppStream information enables the [AppImage] to be discoverable in application centers and/or application directory websites. If it does, then it **MUST** follow the AppStream [guidelines for applications](https://github.com/hughsie/appstream-glib#guidelines-for-applications). See [examples](https://packages.debian.org/search?searchon=contents&keywords=appdata.xml&mode=filename&suite=unstable&arch=any) for such files in the debian repository.

#### Update information

An [AppImage] **MAY** have [update information] embedded for exactly one transport mechanism. The location in which this information is stored is defined by the [image format](#image-format). Currently three transport mechanisms are available, but only one can be used for each given [AppImage]:

 * [`zsync`](#zsync)
 * [`gh-releases-zsync`](#github-releases)
 
*Several tools such as `appimagetool` can create the `.zsync` file automatically when building the AppImage. Please refer to the [guide](https://docs.appimage.org/packaging-guide/optional/updates.html#using-appimagetool) for further information.*
 
##### zsync

The __[zsync]__ transport requires a HTTP server that can handle HTTP range requests. Its [update information] is in the form

```
zsync|https://server.domain/path/Application-latest-x86_64.AppImage.zsync
```

If an [AppImage] has [update information] embedded for this transport mechanism, then the following fields **MUST** be used; separated by a "|" character:

Field | Type | Example | Comments
----------- | ------ | -------- | --------
Transport mechanism | String | `zsync` | [zsync] file and [AppImage] **MUST** be stored on  [compatible](http://zsync.moria.org.uk/server-issues) HTTP server
zsync file URL | String | `https://server.domain/path/Application-latest-x86_64.AppImage.zsync` | URL to the `.zsync` file (URL **MUST NOT** change from version to version)

For an overview about [zsync] and how to create `.zsync` files, see [http://zsync.moria.org.uk/](http://zsync.moria.org.uk/).

##### GitHub Releases

The __GitHub Releases__ transport extends the [zsync] transport in that it uses version information from [GitHub Releases](https://help.github.com/articles/about-releases/). Its [update information] is in the form

```
gh-releases-zsync|probono|AppImages|latest|Subsurface-*x86_64.AppImage.zsync
```

If an [AppImage] has [update information] embedded for this transport mechanism, then the following fields **MUST** be used; separated by a "|" character:

Field | Type | Example | Comments
----------- | ------ | -------- | --------
Transport mechanism | String | `gh-releases-zsync` | [zsync] file and [AppImage] **MUST** be stored on GitHub Releases
GitHub username | String | `probono` | Name of the GitHub user or organization of the account where the [zsync] file and [AppImage] are stored
GitHub repository | String | `AppImages` | Name of the GitHub repository in which the [zsync] file and [AppImage] are stored
Release name | String | `latest` | Name of the release. `latest` will automatically use the latest release as determined by the GitHub API
Filename | String | `Subsurface-*x86_64.AppImage.zsync` | Filename of the [zsync] file on GitHub, `*` is a wildcard

**Please note that pre-releases are not being considered when using `latest`.** You will have to explicitly provide the name of a release. When using e.g., [uploadtool](https://github.com/probonopd/uploadtool), the name of the release created will always be `continuous`, hence, you can just specify that value instead of `latest`.


##### Pling/AppImageHub

[Pling](www.pling.com) is a contents store which allows uploading AppImages. It also includes an AppImage exclusive catalog which is available at [www.appimagehub.com](). This transport method 
extends [zsync] and uses the file name sorting to determine the latest release. Its [update information] is in the form

```
pling-v1-zsync|1623134|*-stable-x86_64.AppImage
```


If an [AppImage] has [update information] embedded for this transport mechanism, then the following fields **MUST** be used; separated by a "|" character:

Field | Type | Example | Comments
----------- | ------ | -------- | --------
Transport mechanism | String | `pling-v1-zsync` | [AppImage] file **MUST** be stored in pling.com
Product ID | String | `1623134` | Id given by pling.com when the product is created.
File Matching Pattern | String | `*-stable-x86_64.AppImage` | [Wildcard matching expression](http://www.gnu.org/software/libc/manual/html_node/Wildcard-Matching.html), **MAY** contain a release channel name. **MUST** contain the target architecture. When sorted by file name they **MUST** follow the release order.

**Please notice** that all file names must follow an specific pattern defined by the packager (you). But it's recommended to include the target application name, the version, the release channel, the architecture and the `.AppImage` extensions as follows: `bash-5.4.0-stable-x86_64.AppImage`. You **MUST NOT** upload the zsync file to the Pling store.

You can visit https://www.pling.com/product/add to register a new product on the Pling store.


##### bintray-zsync

The __bintray-zsync__ transport was there to support [Bintray](https://bintray.com/). It is deprecated.

### Runtime

The AppImage runtime is the program that mounts the filesystem image and executes the payload.

If the AppImage runtime is a static binary, then it **MUST** support the environment variable `TARGET_APPIMAGE`. All runtimes **SHOULD** implement this environment variable. If set to an existing path, then the AppImage Runtime will use the filesystem image from the AppImage at that path for all its operations rather than the filesystem image appended to the runtime binary.

### Desktop integration

The software inside the [AppImage] **MAY** integrate into the desktop environment (e.g., by installing a [.desktop file] into the host system) on the user's behalf. However if it does so, it *SHOULD* ensure to get the explicit permission of the user, e.g., by asking for permission in a GUI window, or by reacting to a command line argument.

The software inside the [AppImage] **SHOULD NOT** attempt to do desktop integration if at least one of the following conditions are met:

* A file `$XDG_DATA_HOME/appimagekit/no_desktopintegration` exists on the [target system] (with `$XDG_DATA_HOME` as defined in the [XDG Base Directory Specification](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html))
* A file `/usr/share/appimagekit/no_desktopintegration` exists on the [target system]
* A file `/etc/appimagekit/no_desktopintegration` exists on the [target system]
* A process named `appimaged` is running on the system
* The environment variable `$DESKTOPINTEGRATION` is not empty
