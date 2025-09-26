# September 26, 2025

I had some difficulties packaging a Kivy/KivyMD app for Android using MacOS, so I hope these tips will help someone.

I am using an M1 MacBook Air (Apple Silicon).

First, I will assume you have installed python==3.11.9, kivy==2.3.1 and kivymd==1.2.0. In addition, you have successfully run your app from the terminal, e.g., python3 main.py, and it is working correctly.

Second, install java JDK version 17. THIS IS VERY VERY VERY IMPORTANT. If you made the mistake of installing JDK 20 or above, you got this weird error during buildozer build.

Next, let's update pip

Third, pip3 install --upgrade pip
Install buildozer and a couple of dependencies.

I think, You should better using virutalenv!!!

mk kivy_venv

cd kivy_venv

python3 -m env .venv

source .venv/bin/activate

pip3 install --upgrade buildozer

python3 -m pip install --upgrade Cython==0.29.33 virutalenv   # THIS IS VERY IMPORTANT USING SPECIFIC VERSION OF Cython LIKE 0.29.33

Now, inside your project's folder, run

buildozer init

buildozer -v android debug(or release)

THIS IS THE LASTEST of buildozer.spec to follow GOOGLE POLISH.(16K of page size)
--------------------------
### This setting is designed to comply with Google’s policy, such as applying a 16KB page size and Android SDK 35 or higher  ###
[app]

title = your app title

package.name = your app name

package.domain = your app domain

source.dir = .

source.include_exts = py,png,jpg,kv,atlas,ttf,json

source.include_patterns = font/*,images/*.png

source.exclude_exts = spec,txt

source.exclude_dirs = tests, bin, venv, __pycache__

version = your app version

# IF YOU ARE USING PYTHON, KIVY, KIVYMD, BS4, REQUESTS, PLYER, PYJNIUS OLEFILE ETC ... 

requirements = python3,kivy==2.3.1,kivymd==1.2.0,sdl2_ttf,pillow,android,beautifulsoup4,soupsieve,requests==2.31.0,typing_extensions,plyer==2.1.0,pyjnius,olefile==0.47

presplash.filename = %(source.dir)s/images/presplash.png  # If you have presplash image

icon.filename = %(source.dir)s/images/favicon.png         # If you have presplash image

orientation = portrait,landscape

osx.python_version = 3

osx.kivy_version = 2.3.1

fullscreen = 0

android.permissions = android.permission.INTERNET, android.permission.ACCESS_NETWORK_STATE, android.permission.VIBRATE, android.permission.WRITE_EXTERNAL_STORAGE, android.permission.FOREGROUND_SERVICE

# Android Version Setting
android.api = 35

android.minapi = 24

android.sdk = 35

android.ndk = 28

android.ndk_api = 24

android.private_storage = True

# 16KB Page Size 
android.archs = arm64-v8a

android.add_linking_options = -Wl,--warn-shared-textrel

# SETTING TO FOLLOW of 16KB Page Size 

p4a.branch = develop

# Gradle Dependent

android.gradle_dependencies = androidx.appcompat:appcompat:1.6.1, androidx.activity:activity:1.6.1

android.enable_androidx = True

android.add_packaging_options = "jniLibs/useLegacyPackaging=true"

# CMake Option Improvement

android.cmake_options = -DCMAKE_POLICY_VERSION_MINIMUM=3.18, -DCMAKE_C_FLAGS="-O2 -fPIC", -DCMAKE_CXX_FLAGS="-O2 -fPIC"

# 16KB Page Size flag

android.add_gradle_options = android.bundle.enableUncompressedNativeLibs=false

# BIG SIZE DISPLAY SUPPORTING

android.allow_backup = True

android.allow_cleartext_traffic = True

[buildozer]

log_level = 0

warn_on_root = 1

[android.manifest]

application.resizeableActivity = true

activity.screenOrientation = unspecified

application.largeHeap = true

application.hardwareAccelerated = true

uses-feature.android.hardware.screen.portrait = false

uses-feature.android.hardware.screen.landscape = false
