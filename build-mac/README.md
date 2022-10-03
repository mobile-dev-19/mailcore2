### Swift Package Manager ###

MailCore 2 tersedia melalui [Swift Package Manager](https://swift.org/package-manager/).

1. Dalam Xcode klik `File` -> `Swift Packages` -> `Add Package Dependency...`
2. Tempel URL berikut: `https://github.com/MailCore/mailcore2`
3. Pada layar `Choose Package Options`, di bawah `Rules` beralih dari `Version` ke `Branch` (`Branch: master`) akan menjadi default
4. Klik `Berikutnya` -> `Berikutnya` -> `Selesai`

**Langkah-langkah berikut adalah untuk menyelesaikan masalah dengan versi Xcode 12 saat ini, setelah masalah diperbaiki, mereka tidak perlu dan dihapus.**

4. Pilih `<YOUR_PROJECT>` -> `<YOUR_TARGET>` -> `Build Phases`
5. Klik `+` -> `New Copy Files Phase`, lalu ubah `Destination` menjadi `Frameworks` di fase build baru
6. Klik `+` -> `New Run Script Phase`, lalu tempel kotak skrip:
```
if [ "$PLATFORM_NAME" == "macosx" ]
then
    FRAMEWORK_PATH="$CODESIGNING_FOLDER_PATH"/Contents/Frameworks/MailCore.framework/Versions/A/MailCore
else
    FRAMEWORK_PATH="$CODESIGNING_FOLDER_PATH"/Frameworks/MailCore.framework
fi
echo "Signing framework at: $FRAMEWORK_PATH"
/usr/bin/codesign --force --sign ${EXPANDED_CODE_SIGN_IDENTITY} --preserve-metadata=identifier,entitlements "$FRAMEWORK_PATH"
```
[![Swift Package Manger Compatible](https://img.shields.io/badge/SPM-compatible-4BC51D.svg?style=flat)](https://swift.org/package-manager/)

### Carthage ###

MailCore 2 is available through [Carthage](https://github.com/Carthage/Carthage).

Add this line to the `Cartfile`
```
github "MailCore/mailcore2"
```

Here you can find [Instructions for install Carthage](https://github.com/Carthage/Carthage#installing-carthage).

[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)

### CocoaPods ###

MailCore 2 is available on [CocoaPods](http://cocoapods.org/).

**For iOS:**
```
pod 'mailcore2-ios'
```

**For OS X:**
```
pod 'mailcore2-osx'
```

[![CocoaPods Compatible](https://img.shields.io/badge/CocoaPods-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)

### Binary ###

**For iOS:**
Download the latest [build for iOS](http://d.etpan.org/mailcore2-deps/mailcore2-ios/).

**For OS X:**
Download the latest [build for OS X](http://d.etpan.org/mailcore2-deps/mailcore2-osx/).

### Build for iOS/OSX ###

1. Jika Anda bermigrasi dari MailCore1, Anda harus terlebih dahulu membersihkan folder build Anda.
2. Checkout MailCore2 ke dalam direktori relatif terhadap proyek Anda.
3. Di bawah direktori `build-mac`, cari file `mailcore2.xcodeproj`, dan seret ini ke proyek Xcode Anda.
4. **For Mac** - If you're building for Mac, you can either link against MailCore 2 as a framework, or as a static library:
    * Mac framework
        - Go to Build Phases from your build target, and under 'Link Binary With Libraries', add `MailCore.framework` and `Security.framework`.
        - Make sure to use LLVM C++ standard library.  Open Build Settings, scroll down to 'C++ Standard Library', and select `libc++`.
        - In Build Phases, add a Target Dependency of `mailcore osx` (it's the one with a little toolbox icon).
        - Goto `Editor > Add Build Phase > Copy Files`.
        - Expand the newly created Build Phase and change it's destination to "Frameworks".
        - Click the `+` icon and select `MailCore.framework`.
    * Mac static library
        - Go to Build Phases from your build target, and under 'Link Binary With Libraries', add `libMailCore.a` and `Security.framework`.
        - Set 'Other Linker Flags' under Build Settings: `-lctemplate -letpan -lxml2 -lsasl2 -liconv -ltidy -lz` `-lc++ -stdlib=libc++ -ObjC -lresolv`
        - Make sure to use LLVM C++ standard library.  In Build Settings, locate 'C++ Standard Library', and select `libc++`.
        - In Build Phases, add a Target Dependency of `static mailcore2 osx`.
5. **For iOS** - If you're targeting iOS, you have to link against MailCore 2 as a static library:
    * Add `libMailCore-ios.a`
    * Add `CFNetwork.framework`
	* Add `Security.framework`
    * Set 'Other Linker Flags': `-lctemplate-ios -letpan-ios -lxml2 -lsasl2 -liconv -ltidy -lz` `-lc++  -lresolv -stdlib=libc++ -ObjC`
    * Make sure to use LLVM C++ standard library.  Open Build Settings, scroll down to 'C++ Standard Library', and select `libc++`.
    * In Build Phases, add a Target Dependency of `static mailcore2 ios`.
6. **For Swift** - If you are using Mailcore in a Swift project you also need to complete the following steps:
    * Create a new header file in your project and name it ```Project-Name-Bridging-Header.h```.
    * Remove any template code from the file and add ```#import <MailCore/MailCore.h>```
    * In your target settings search for _Objective-c Bridging Header_ and add a link to your bridging header.
    * You do not need to import Mailcore in any of your classes as the bridging header takes care of this automatically.
7. Profit.

Here's a video that shows all the steps for iOS:
http://www.youtube.com/watch?v=9fAo6oBzlQI
