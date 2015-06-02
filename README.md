<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
 

- [dreamcat4/tvh.ubuntu.build.*](#dreamcat4tvhubuntubuild)
  - [Authoring packages for other Distros](#authoring-packages-for-other-distros)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

![License MIT](https://img.shields.io/badge/license-MIT-blue.svg)

## dreamcat4/tvh.ubuntu.build.*
**_Builders for Ubuntu .deb binary pkgs of Tvheadend server_**

Reference guide for package maintainers, starts here:

- [dreamcat4/tvh.ubuntu.build.*](ubuntu/0. maintainers-guide.md)

The guide above ^^ is for ubuntu. But most of the steps are similar for other distros.

### Authoring packages for other Distros

There are now packages for ubuntu. But what about Debian, openSUSE, Fedora, CentOS, Arch and whatever other distros ?

For this example, we use the `centos` distribution. We assume you are also a user of `centos` who is familiar with it's package manager.

* Copy the whole folder, from `ubuntu/` --> `centos/` etc.
  * `git clone https://github.com/tvheadend/tvheadend-build.git`
  * `cd tvheadend-build && cp -rf ubuntu centos`

* In the new folder, find the dependancies base image `deps/Dockerfile`
  * Change the 1st line:
    * `FROM: ubuntu-debootstrap:14.04` ---> a decent minimal official docker base image of `centos` or `debian` etc. whatever.
  * Change the `apt-get install` ---> commands specific to your distro's package manager e.g. `yum`.

* In the remaining folders: `master/`, `unstable`, `testing` and `stable/` open the `Dockerfile`: 
  * Change the 1st line:
    * `FROM: dreamcat4/tvh.build.ubuntu.deps` ---> `FROM: yourDockerUsername/tvh.build.centos.deps`
  * Change the `Autobuild.sh` line, which builds a `.deb` file, to build an rpm instead.
    * Try to keep all the same `./configure` args such as `----enable-libffmpeg_static` etc.
  * At bottom, we run a script to upload the `/out` files to a bintray repo:
    * You will need modify that script `upload-to-bintray` also.

Follow all the the other steps in the Ubuntu guide - it is mostly the same. Just with different repo names, username, and so on.

* Commit changes to git.
* Push the new `centos` folder up to github.

### Cross-compiling for ARM

The latest official arch linux package of tvheadend:

* https://aur.archlinux.org/packages/tvheadend/

Some useful links for cross-compiling:

* http://vraidsys.com/2013/06/cross-compile-tvheadend-for-arm-cortex-a9/
* https://github.com/jzerbe/jzerbe.github.com/blob/master/_posts/2013-06-22-cross-compile-tvheadend-for-arm-cortex-a9.md
* http://www.holik.at/index.php?m=06&y=13&entry=entry130627-175237

Docker hub is an x86 build farm. Tvheadend would need to be cross compiled (intel host) to be built on those servers.

