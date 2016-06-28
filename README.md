# google-chrome-build
Google chrome build for centos
Packages both google-chrome-stable and chrome-deps-stable

[![Build Status](https://snap-ci.com/snap-ci/google-chrome-build/branch/master/build_image)](https://snap-ci.com/snap-ci/google-chrome-build/branch/master)

<pre>
yum install rpm2cpio cpio redhat-lsb wget xdg-utils GConf2 libXScrnSaver libX11 gnome-keyring nss PackageKit rpm-build rpmdevtools xz
bundle install --path .bundle
bundle exec rake
</pre>
