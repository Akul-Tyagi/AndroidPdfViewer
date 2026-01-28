# Android PdfViewer — 16KB Page Aligned & Modernized

<p align="center">
  <a href="https://jitpack.io/#Akul-Tyagi/AndroidPdfViewer"><img src="https://jitpack.io/v/Akul-Tyagi/AndroidPdfViewer.svg" alt="JitPack"></a>
  <img src="https://img.shields.io/badge/Android%20API-21%2B-brightgreen.svg" alt="API 21+">
  <img src="https://img.shields.io/badge/Android%2015-Compatible-blue.svg" alt="Android 15 Compatible">
  <img src="https://img.shields.io/badge/16KB%20Page-Compliant-success.svg" alt="16KB Page Compliant">
  <img src="https://img.shields.io/badge/License-Apache%202.0-blue.svg" alt="License">
</p>

> **One of the first globally modernized forks of AndroidPdfViewer & PdfiumAndroid libraries with 16KB page size support and rebuilt 64-bit native libraries to comply with Google Play 2025 policies and Android 15 kernel mandates.**

A high-performance PDF rendering library for Android with `animations`, `gestures`, `zoom`, and `double tap` support. This fork uses a custom [PdfiumAndroid](https://github.com/Akul-Tyagi/PdfiumAndroid) with recompiled native C++ libraries enforcing **16KB page-size alignment** — critical for forward compatibility with Android 15+ and Google Play Store requirements.

---

## 🚀 Why This Fork?

In 2024, Google announced that **all apps on the Play Store must support 16KB memory page sizes** or face removal. This requirement stems from Android 15's kernel changes supporting devices with 16KB page sizes (vs. traditional 4KB).

At the time, the original AndroidPdfViewer and PdfiumAndroid libraries were unmaintained and non-compliant. Rather than risk app removal, this fork was created to:

- ✅ **Rebuild native C++ libraries** with 16KB page alignment (`-Wl,-z,max-page-size=16384`)
- ✅ **Modernize the entire build toolchain** (AGP 8.12.0, Gradle 8.14.2, Java 17)
- ✅ **Update to Android 15** (API 35) compile and target SDK
- ✅ **Ensure forward compatibility** with Google Play 2025 policies
- ✅ **Optimize memory footprint** through modernized native library compilation

### Real-World Validation

This fork has been validated in production applications serving:
- **700+ active users** across **50+ countries**
- **20,000+ sessions** with **99.8% crash-free rate**

*(Metrics verified via Firebase Analytics & Android Vitals as of 2024)*

---

## 📦 Installation

### Step 1: Add JitPack Repository

Add JitPack to your project's `settings.gradle` (recommended) or root `build.gradle`:

```groovy
// settings.gradle (Gradle 7.0+)
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
        maven { url 'https://jitpack.io' }
    }
}
```

Or for older projects using `build.gradle`:

```groovy
// root build.gradle
allprojects {
    repositories {
        google()
        mavenCentral()
        maven { url 'https://jitpack.io' }
    }
}
```

### Step 2: Add Dependency

Add the library to your module's `build.gradle`:

```groovy
dependencies {
    implementation 'com.github.Akul-Tyagi:AndroidPdfViewer:v3.4.3-16kb'
}
```

---

## ✨ What's New in v3.4.3-16kb

### 🔧 Critical Modernization
| Component | Before | After |
|-----------|--------|-------|
| **Page Size Support** | 4KB only | **16KB aligned** ✅ |
| **Compile SDK** | 28 | **35** (Android 15) |
| **Target SDK** | 28 | **35** (Android 15) |
| **Min SDK** | 19 | **21** (library), 19 (sample) |
| **AGP Version** | 3.x | **8.12.0** |
| **Gradle Version** | 5.x | **8.14.2** |
| **Java Version** | 8 | **17** |
| **AndroidX** | Partial | **Full Migration** |
| **Jetifier** | Required | **Disabled** |

### 🛠 Native Library Changes
- Rebuilt all native C++ libraries with 16KB page alignment
- Updated to PdfiumAndroid fork: [`com.github.Akul-Tyagi:PdfiumAndroid:v1.11.4-16kb`](https://github.com/Akul-Tyagi/PdfiumAndroid)
- Supports both 32-bit and 64-bit architectures (armeabi-v7a, arm64-v8a, x86, x86_64)
- Compliant with Google Play 64-bit requirement

### 📋 Inherited Features (from upstream)
- All features from AndroidPdfViewer 3.2.0-beta.3
- Optimized page loading (PR #714)
- Fixed max/min zoom levels (PR #776)
- Fixed memory leaks (PR #702)
- Thread management improvements (PR #703)
- `fitEachPage` option (PR #627)
- Long press disable option (PR #689)

---

## 🔧 Technical Details

### 16KB Page Size Compliance

Android 15 introduces support for devices with 16KB memory page sizes. Apps with native libraries must be compiled with proper alignment:

```bash
# Native libraries must be built with:
-Wl,-z,max-page-size=16384
```

This fork's [PdfiumAndroid](https://github.com/Akul-Tyagi/PdfiumAndroid) dependency has been rebuilt from source with these flags, ensuring:
- ✅ Compatibility with 16KB page size devices
- ✅ Backward compatibility with 4KB devices
- ✅ Google Play Store compliance

### Build Configuration

```groovy
// Root build.gradle
ext {
    versions = [
        agp        : "8.12.0",
        minSdk     : 21,
        targetSdk  : 35,
        compileSdk : 35,
        viewerVer  : "3.4.3-16kb"
    ]
}
```

### Gradle Wrapper
```properties
distributionUrl=https\://services.gradle.org/distributions/gradle-8.14.2-bin.zip
```

---

## 📖 Usage

### Include PDFView in Layout

```xml
<com.github.barteksc.pdfviewer.PDFView
    android:id="@+id/pdfView"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
```

### Load a PDF Document

```java
pdfView.fromUri(Uri)
// or
pdfView.fromFile(File)
// or
pdfView.fromBytes(byte[])
// or
pdfView.fromStream(InputStream)
// or
pdfView.fromSource(DocumentSource)
// or
pdfView.fromAsset(String)
    .pages(0, 2, 1, 3, 3, 3) // all pages are displayed by default
    .enableSwipe(true) // allows to block changing pages using swipe
    .swipeHorizontal(false)
    .enableDoubletap(true)
    .defaultPage(0)
    .onDraw(onDrawListener) // draw on current page
    .onDrawAll(onDrawListener) // draw on all pages
    .onLoad(onLoadCompleteListener) // called after document is loaded
    .onPageChange(onPageChangeListener)
    .onPageScroll(onPageScrollListener)
    .onError(onErrorListener)
    .onPageError(onPageErrorListener)
    .onRender(onRenderListener) // called after document is rendered
    .onTap(onTapListener)
    .onLongPress(onLongPressListener)
    .enableAnnotationRendering(false)
    .password(null)
    .scrollHandle(null)
    .enableAntialiasing(true)
    .spacing(0) // spacing in dp
    .autoSpacing(false)
    .linkHandler(DefaultLinkHandler)
    .pageFitPolicy(FitPolicy.WIDTH)
    .fitEachPage(false)
    .pageSnap(false)
    .pageFling(false)
    .nightMode(false)
    .load();
```

### Kotlin Example

```kotlin
pdfView.fromAsset("sample.pdf")
    .defaultPage(0)
    .enableSwipe(true)
    .swipeHorizontal(false)
    .onPageChange { page, pageCount -> 
        title = "Page ${page + 1} / $pageCount"
    }
    .onError { throwable ->
        Log.e("PDFView", "Error loading PDF", throwable)
    }
    .scrollHandle(DefaultScrollHandle(this))
    .spacing(10)
    .load()
```

> **Note:** The above Kotlin example uses SAM conversions. If your project doesn't support SAM conversions for Java interfaces, use explicit listener implementations:
> ```kotlin
> .onPageChange(OnPageChangeListener { page, pageCount -> ... })
> ```

---

## 🎛 Features

### Scroll Handle

```java
// Default scroll handle (right side for vertical, bottom for horizontal)
.scrollHandle(new DefaultScrollHandle(this))

// Left/top position
.scrollHandle(new DefaultScrollHandle(this, true))
```

### Document Sources

```java
pdfView.fromUri(Uri)      // Content provider URIs
pdfView.fromFile(File)    // Local files
pdfView.fromBytes(byte[]) // Byte arrays
pdfView.fromStream(InputStream)
pdfView.fromAsset(String) // Assets folder
pdfView.fromSource(DocumentSource) // Custom sources
```

### Link Handling

```java
// Default behavior: internal links jump to page, external links open in browser
.linkHandler(DefaultLinkHandler)

// Custom handler
.linkHandler(new LinkHandler() {
    @Override
    public void handleLinkEvent(LinkTapEvent event) {
        // Custom link handling
    }
})
```

### Page Fit Policies

```java
.pageFitPolicy(FitPolicy.WIDTH)  // Fit width (default)
.pageFitPolicy(FitPolicy.HEIGHT) // Fit height
.pageFitPolicy(FitPolicy.BOTH)   // Fit both dimensions
```

### ViewPager-like Behavior

```java
.swipeHorizontal(true)
.pageSnap(true)
.autoSpacing(true)
.pageFling(true)
```

### Bitmap Quality

```java
// Default: RGB_565 (lower memory)
// For higher quality:
pdfView.useBestQuality(true); // Uses ARGB_8888
```

### Zoom Levels

```java
pdfView.setMinZoom(1.0f);   // Default: 1.0
pdfView.setMidZoom(1.75f);  // Default: 1.75
pdfView.setMaxZoom(3.0f);   // Default: 3.0
```

---

## 🔒 ProGuard / R8

Add to your ProGuard rules:

```proguard
-keep class com.shockwave.**
```

This keeps the Pdfium native interface classes required for JNI calls.

---

## ❓ FAQ

### Why is the APK so large?

PdfiumAndroid contains native libraries for multiple architectures (~16MB total). Use APK splits to reduce size:

```groovy
android {
    splits {
        abi {
            enable true
            reset()
            include 'arm64-v8a', 'armeabi-v7a', 'x86', 'x86_64'
            universalApk false
        }
    }
}
```

### How to load PDF from URL?

This library doesn't handle downloads. Download the file first, then use `fromFile()` or `fromBytes()`.

### How to restore page position after rotation?

```java
@Override
protected void onSaveInstanceState(Bundle outState) {
    super.onSaveInstanceState(outState);
    outState.putInt("page", pdfView.getCurrentPage());
}

@Override
protected void onCreate(Bundle savedInstanceState) {
    // ...
    int page = savedInstanceState != null ? savedInstanceState.getInt("page", 0) : 0;
    pdfView.fromAsset("doc.pdf")
        .defaultPage(page)
        .load();
}
```

---

## 📚 Related Projects

- **[PdfiumAndroid (16KB Fork)](https://github.com/Akul-Tyagi/PdfiumAndroid)** — The companion native PDF rendering library with 16KB page alignment
- [Original AndroidPdfViewer](https://github.com/barteksc/AndroidPdfViewer) — The upstream project (unmaintained)
- [Original PdfiumAndroid](https://github.com/barteksc/PdfiumAndroid) — The upstream native library (unmaintained)

---

## 📄 Changelog

### v3.4.3-16kb (2024)
- 🎯 **16KB page size compliance** — Critical for Android 15 and Play Store 2025
- ⬆️ Compile/Target SDK updated to 35 (Android 15)
- ⬆️ AGP updated to 8.12.0
- ⬆️ Gradle updated to 8.14.2
- ⬆️ Java 17 support
- ⬆️ Min SDK raised to 21
- 🔗 PdfiumAndroid dependency updated to 16KB-compliant fork
- ✅ Full AndroidX migration
- ✅ Jetifier disabled

See [CHANGELOG.md](CHANGELOG.md) for full version history from upstream.

---

## 🤝 Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Submit a pull request

---

## 📜 License

```
Copyright 2017 Bartosz Schiller
Copyright 2024 Akul Tyagi (16KB modernization fork)

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

---

## 🙏 Acknowledgments

- [Bartosz Schiller](https://github.com/barteksc) — Original AndroidPdfViewer author
- [Joan Zapata](http://joanzapata.com/) — android-pdfview inspiration
- [Min Hiew](https://github.com/mhiew) — Previous maintenance work

---

<p align="center">
  <b>⭐ Star this repo if it helped you comply with Google Play 2025 requirements! ⭐</b>
</p>


