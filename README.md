# Blockstream Greener - A native bitcoin wallet for Android and [iOS](https://github.com/Blockstream/green_ios)

## It's literally Green but with satoshis support for Bitcoin denominations

Build status: [![Build Status](https://travis-ci.org/Blockstream/green_android.png?branch=master)](https://travis-ci.org/Blockstream/green_android)

<a href="https://f-droid.org/packages/com.greenaddress.greenbits_android_wallet/" target="_blank">
<img src="https://f-droid.org/badge/get-it-on.png" alt="Get it on F-Droid" height="90"/></a>
<a href="https://play.google.com/store/apps/details?id=com.greenaddress.greenbits_android_wallet" target="_blank">
<img src="https://play.google.com/intl/en_us/badges/images/generic/en-play-badge.png" alt="Get it on Google Play" height="90"/></a>

## Build requirements

You need to have the following Android developer tools installed:

- "Android SDK Platform-tools" version 28.0.2 recommended
- "Android SDK Tools" version 26.1.1
- "Android SDK Build-tools" version 28.0.3
- "Android Support Library" version 28.0.0

The above tools can be installed from the Android SDK manager.

## Clone the repo

`git clone https://github.com/Blockstream/green_android.git`

`cd green_android`

## How to build

#### Use the released native libraries (recommended):

To fetch and install the pre-built naitive libraries, run:

`./app/fetch_gdk_binaries.sh`

The pre-built native libraries are the same versions used in the builds
published by GreenAddress.

#### Cross-compile the native libraries (advanced):

If you wish to make changes to the native libraries or would like to build
completely from source, you can compile
[gdk](https://github.com/Blockstream/gdk)
from its source code yourself.

This requires the "Android NDK" (version r19b recommended) from Android
developer tools, as well as [SWIG](http://www.swig.org/). Most Linux
distributions have SWIG available, on debian for example you can install it
using:

`sudo apt-get install swig`

You must set the environment variables `ANDROID_NDK` and `JAVA_HOME`
correctly, then run:

`cd app && ./prepare_gdk_clang.sh && cd ..`

If you get errors building please ensure your are using the recommended NDK
version.

#### Build the Android app

Run:

`./gradlew build`

This will build both release and debug builds.

You can speed up builds by limiting the tasks which run. Use:

`./gradlew --tasks`

To see a list of available tasks.

#### Rebuild the checkpoints (optional)

Checkpoint files reduce the amount of data that SPV has to download. The
checkpoint data is rebuilt periodically but you may wish to update it if
you will be making and testing changes.

To rebuild, start both MAINNET and TESTNET instances of bitcoind on
localhost. Make sure they are fully synchronized and have finished
booting (verifying blocks, etc).

On MAINNET:

`./gradlew --project-dir=bitcoinj/tools buildMainnetCheckpoints && mv bitcoinj/tools/checkpoints app/src/main/assets/production/checkpoints`


On TESTNET:

`./gradlew --project-dir=bitcoinj/tools buildTestnetCheckpoints && mv bitcoinj/tools/checkpoints-testnet app/src/main/assets/btctestnet/checkpoints`

Or to build both at once, run:

`./buildCheckpoints.sh`

#### Rebuilding with docker (optional)

If you have docker configured and want to build the app in release mode
without having to set up an Android development environment, run:

```
cd contrib
docker build -t greenbits_docker .
docker run -v $PATH_TO_GREENBITS_REPO:/gb greenbits_docker
```

If you don't need to build the Docker image, you can instead run:

`docker pull greenaddress/android && docker run -v $PATH_TO_GREENBITS_REPO:/gb greenaddress/android`


### Acknowledgements

Thanks to [Bitcoin Wallet for Android](https://github.com/schildbach/bitcoin-wallet) for their QR scanning activity source code!
