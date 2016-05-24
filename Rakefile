require 'rubygems'
require 'bundler/setup'
require 'rake/clean'

distro = 'rpm'
version = 'current'
fpm_opts = " --rpm-user root --rpm-group root "

class RpmSpec < Struct.new(:version, :release, :conf_dir, :conf_dir_includes)
  def get_binding
    binding
  end
end

namespace :chrome do
  release = Time.now.utc.strftime('%Y%m%d%H%M%S')
  chrome_rpm = 'google-chrome-stable_current_x86_64.rpm'
  download_url = "https://dl.google.com/linux/direct/#{chrome_rpm}"
  jailed_root = File.expand_path('../src', __FILE__)
  prebuilt_lib = 'libstdc++.so.6'

  CLEAN.include("downloads")
  CLEAN.include("jailed-root")
  CLEAN.include("log")
  CLEAN.include("pkg")
  CLEAN.include("src")

  task :init do
    mkdir_p "log"
    mkdir_p "pkg"
    mkdir_p "downloads"
    mkdir_p "jailed-root"
    mkdir_p "src"
  end

  task :download do
    cd 'downloads' do
      sh("curl --fail --location #{download_url} > #{chrome_rpm} 2>/dev/null")
    end
  end

  task :unpack_rpm do
    cd 'src' do
      sh("rpm2cpio ../downloads/#{chrome_rpm} | cpio -idmv")
    end
  end

  task :prebuilt_libs do
    cd 'src' do
      sh("mkdir opt/google/chrome/lib")
    end
  end

  task :repackage_rpm do
    require 'erb'

    ERB.new(File.read(File.expand_path('../google-chrome-stable.spec.erb', __FILE__)), nil , '-').tap do |template|
      File.open("/tmp/google-chrome-stable.spec", 'w') do |f|
        attrs = RpmSpec.new(version, release)
        f.puts(template.result(attrs.get_binding))
      end
      at_exit {rm_rf "/tmp/google-chrome-stable.spec"}
    end

    cd jailed_root do
      output_dir = File.expand_path('../target-rpms', __FILE__)
      at_exit {rm_rf output_dir}
      puts "*** Building RPM..."
      rpmbuild_cmd = []
      rpmbuild_cmd << "rpmbuild /tmp/google-chrome-stable.spec"
      rpmbuild_cmd << "--verbose"
      rpmbuild_cmd << "--buildroot #{jailed_root}"
      rpmbuild_cmd << "--define '_tmppath #{jailed_root}/../rpm_tmppath'"
      rpmbuild_cmd << "--define '_topdir #{output_dir}'"
      rpmbuild_cmd << "--define '_rpmdir #{output_dir}'"
      rpmbuild_cmd << "--define 'noclean 1'"
      rpmbuild_cmd << "-bb"

      sh rpmbuild_cmd.join(" ")
    end
    sh("mv target-rpms/x86_64/*.rpm pkg/")
  end

  task :package => [:clean, :init, :download, :unpack_rpm, :prebuilt_libs, :repackage_rpm]
end

desc 'build google-chrome for centos'
task :default => 'chrome:package'
