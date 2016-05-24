# google-chrome-build
Google chrome build for centos

<pre>
yum innstall rpm2cpio cpio redhat-lsb wget xdg-utils GConf2 libXScrnSaver libX11 gnome-keyring nss PackageKit $rpm_build_packages rpm-build rpmdevtools xz
bundle install --path .bundle
bundle exec rake
</pre>
