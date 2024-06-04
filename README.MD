![](docs/images/logo.png)

[![Downloads](https://img.shields.io/badge/dynamic/json?color=green&label=Downloads&query=totalString&url=https%3A%2F%2Fraw.githubusercontent.com%2Ftopjohnwu%2Fmagisk-files%2Fcount%2Fcount.json&cacheSeconds=1800)](https://raw.githubusercontent.com/topjohnwu/magisk-files/count/count.json)

#### This is not an officially supported Google product

## Introduction

Magisk is a suite of open source software for customizing Android, supporting devices higher than Android 6.0.<br>
Some highlight features:

- **MagiskSU**: Provide root access for applications
- **Magisk Modules**: Modify read-only partitions by installing modules
- **MagiskBoot**: The most complete tool for unpacking and repacking Android boot images
- **Zygisk**: Run code in every Android applications' processes

## Downloads

[Github](https://github.com/topjohnwu/Magisk/) is the only source where you can get official Magisk information and downloads.

[![](https://img.shields.io/badge/Magisk-v27.0-blue)](https://github.com/topjohnwu/Magisk/releases/tag/v27.0)
[![](https://img.shields.io/badge/Magisk%20Beta-v27.0-blue)](https://github.com/topjohnwu/Magisk/releases/tag/v27.0)
[![](https://img.shields.io/badge/Magisk-Canary-red)](https://raw.githubusercontent.com/topjohnwu/magisk-files/canary/app-release.apk)
[![](https://img.shields.io/badge/Magisk-Debug-red)](https://raw.githubusercontent.com/topjohnwu/magisk-files/canary/app-debug.apk)

## Useful Links

- [Installation Instruction](https://topjohnwu.github.io/Magisk/install.html)
- [Building and Development](https://topjohnwu.github.io/Magisk/build.html)
- [Magisk Documentation](https://topjohnwu.github.io/Magisk/)

## Bug Reports

**Only bug reports from Debug builds will be accepted.**

For installation issues, upload both boot image and install logs.<br>
For Magisk issues, upload boot logcat or dmesg.<br>
For Magisk app crashes, record and upload the logcat when the crash occurs.

## Translation Contributions

Default string resources for the Magisk app and its stub APK are located here:

- `app/src/main/res/values/strings.xml`
- `stub/src/main/res/values/strings.xml`

Translate each and place them in the respective locations (`[module]/src/main/res/values-[lang]/strings.xml`).

## License

    Magisk, including all git submodules are free software:
    you can redistribute it and/or modify it under the terms of the
    GNU General Public License as published by the Free Software Foundation,
    either version 3 of the License, or (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    You should have received a copy of the GNU General Public License
    along with this program.  If not, see <http://www.gnu.org/licenses/>.

{
  "totalString": "273,378,172",
  "total": 273378172,
  "type": {
    "apk": 189995213,
    "zip": 83382959
  },
name: Magisk Setup
runs:
  using: "composite"
  steps:
    - name: Set up JDK 17
      uses: actions/setup-java@v4
      with:
        distribution: "temurin"
        java-version: "17"

    - name: Set up Python 3
      uses: actions/setup-python@v5
      with:
        python-version: "3.x"

    - name: Set up sccache
      uses: hendrikmuhs/ccache-action@v1.2
      with:
        variant: sccache
        key: ${{ runner.os }}-${{ github.sha }}
        restore-keys: ${{ runner.os }}
        max-size: 10000M

    - name: Cache Gradle dependencies
      uses: actions/cache@v4
      with:
        path: |
          ~/.gradle/caches
          ~/.gradle/wrapper
          !~/.gradle/caches/build-cache-*
        key: ${{ runner.os }}-gradle-${{ hashFiles('**/*.gradle.kts') }}
        restore-keys: ${{ runner.os }}-gradle-

    - name: Cache build cache
      uses: actions/cache@v4
      with:
        path: |
          ~/.gradle/caches/build-cache-*
        key: ${{ runner.os }}-build-cache-${{ github.sha }}
        restore-keys: ${{ runner.os }}-build-cache-

    - name: Set up NDK
      run: python build.py -v ndk
      shell: bash
