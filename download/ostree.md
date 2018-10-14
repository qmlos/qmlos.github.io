---
layout: post
title: Fedora Silverblue
permalink: /download/silverblue/
theme: deep_orange-light_blue
navitem: nav-download
---

Liri OS is available as in immutable tree with reliable updates
and easy rollbacks.

Right now the live image installs a traditional, package-based system,
but if you are running Fedora Silverblue you can try the rpm-ostree based
version of Liri OS today.

First of all add the remote and pull the latest version:

```sh
ref=lirios/unstable/x86_64/desktop
ostree remote add --no-gpg-verify liri https://repo.liri.io/ostree/repo/
ostree pull liri:$ref
ostree admin os-init lirios
ostree admin deploy --os=lirios --karg-proc-cmdline liri:$ref
checksum=$(ostree rev-parse $ref)
for i in /etc/passwd /etc/group /etc/shadow /etc/fstab /etc/default/grub /etc/locale.conf /etc/ostree/remotes.d/liri.conf; do cp $i /ostree/deploy/lirios/deploy/${checksum}.0/$i; done
```

If you have a separate `/home` mount point, you'll need to change that fstab copy to refer to `/var/home`.

If you don't have a separate `/home` mount point, then you need to make sure that a symlink will be created:

```sh
echo 'L /var/home - - - - ../sysroot/home' > /ostree/deploy/lirios/deploy/${checksum}.0/etc/tmpfiles.d/00rpm-ostree.conf
```

and copy your home directory from the `fedora-workstation` deployment:

```sh
cp -a /sysroot/ostree/deploy/fedora-workstation/var/home/<USERNAME> /sysroot/ostree/deploy/lirios/var/home
```

(Replace `<USERNAME>` with your actual username)