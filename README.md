# obs-browser

CEF Based obs-studio browser plugin

## JS Bindings

obs-browser provides a global object that allows access to some obs specific functionality from javascript.

### Get OBS Studio Browser Plugin Version
```
window.obsstudio.pluginVersion
// => 1.24.0
```

### Register for visibility callbacks
```
/**
 * onVisibilityChange gets callbacks when the visibility of the browser source changes in OBS
 *
 * @param {bool} visiblity - True -> visible, False -> hidden
 */
window.obsstudio.onVisibilityChange = function(visiblity) {
	
};
```

### Register for scene change callbacks
```
/**
 * onSceneChange gets callbacks when the scene is changed
 *
 * @param {string} sceneName - The name of the scene that was changed to.
 */
window.obsstudio.onSceneChange = function(sceneName) {
	
};
```

## Building on OSX

### Building CEF
#### Getting
*  Download CEF Mac 64 from [https://cefbuilds.com/](https://cefbuilds.com/)
  *  At the time of writing this I used build 2704
*  Extract and cd into the folder

#### Setting Up Project
```
mkdir build
cmake -D CMAKE_XCODE_ATTRIBUTE_CLANG_CXX_LIBRARY=libc++ -G Xcode ..
open cef.xcodeproj/
```

#### Building
Build in Xcode (⌘+B)

TODO: tell user to move stuff, or update FindCEF.cmake

### Building OBS and obs-browser
#### Installing Dependencies
```
brew install ffmpeg x264 qt5 cmake
```

#### Setting Up Project
```
git clone --recursive https://github.com/jp9000/obs-studio.git
cd ./obs-studio
git clone git@github.com:kc5nra/obs-browser.git ./plugins/obs-browser
echo "add_subdirectory(obs-browser)" >> ./plugins/CMakeLists.txt
mkdir build
cd ./build
cmake -D CMAKE_PREFIX_PATH=/usr/local/opt/qt5/lib/cmake -D CEF_ROOT_DIR=/Users/username/Development/cef_binary_3.2704.1434.gec3e9ed_macosx64 -G Xcode ..
open obs-studio.xcodeproj/
```

#### Building
Build in Xcode (⌘+B)

## Building on Windows

### Building CEF
#### Getting
*  Download CEF Windows 64bit from [https://cefbuilds.com/](https://cefbuilds.com/)
  *  At the time of writing this I used build 2704
*  Extract and cd into the folder

#### Setting Up Project
* Run cmake-gui
  * In "where is the source code", enter in the repo directory (example: C:/Users/User/Desktop/cef_binary_3.2743.1445.gdad7c0a_windows64).
  * In "where to build the binaries", enter the repo directory path with the 'build' subdirectory (example: C:/Users/User/Desktop/cef_binary_3.2743.1445.gdad7c0a_windows64/build).
* Press 'Configure' and select the generator that fits to your installed VS Version:
Visual Studio 12 2013 Win64 or Visual Studio 14 2015 Win64
* Press 'Generate' to generate Visual Studio project files in the 'build' subdirectory.
* Open cef.sln from the 'build' subdirectory

#### Building
Build in Visual Studio

You should fix CEF DLL wrapper path. So either you can
* Copy libraries to a correct location,  
  assuming that CEF_ROOT_DIR is a CEF repo directory path (example: C:/Users/User/Desktop/cef_binary_3.2743.1445.gdad7c0a_windows64)
  * Create directory ${CEF_ROOT_DIR}/build/libcef_dll
  * Copy files from ${CEF_ROOT_DIR}/build/libcef_dll_wrapper/{BUILDTYPE} to ${CEF_ROOT_DIR}/build/libcef_dll
* *OR* change FindCEF.cmake file in the obs-browser repo directory path

### Building OBS and obs-browser
#### Follow the OBS build instructions
[https://github.com/jp9000/obs-studio/wiki/Install-Instructions#windows](https://github.com/jp9000/obs-studio/wiki/Install-Instructions#windows)

#### Setting Up Project
* Add add_subdirectory(obs-browser) to ./plugins/CMakeLists.txt
* Set the CEF_ROOT_DIR path in cmake-gui for obs-studio (example: C:/Users/User/Desktop/cef_binary_3.2743.1445.gdad7c0a_windows64)
* * Press 'Generate' to generate Visual Studio project files in the 'build' subdirectory.
* Open obs-studio.sln from the 'build' subdirectory

#### Building
Build in Visual Studio
