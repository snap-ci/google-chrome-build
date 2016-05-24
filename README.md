# google-chrome-build
Google chrome build for centos
Packages both google-chrome-stable and chrome-deps-stable

<pre>
yum install rpm2cpio cpio redhat-lsb wget xdg-utils GConf2 libXScrnSaver libX11 gnome-keyring nss PackageKit rpm-build rpmdevtools xz
bundle install --path .bundle
bundle exec rake
</pre>
