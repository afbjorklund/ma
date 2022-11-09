# ma - distribution minimalism

Small distribution for constructing container images.

![ma character](assets/ma.png)

There is no traditional package manager _inside_ the target image.
Instead, the software is built using a host build system.

This means that the resulting image can stay "minimalistic".
While the build image contains all the tools needed to make it.

```
REPOSITORY       TAG       IMAGE ID       CREATED      SIZE
buildroot/base   latest    b8817d0eaa5e   3 days ago   975MB
buildroot/root   latest    9344bf52a6d5   3 days ago   1.46MB
```

The basic distro only consists of two components:
- uclibc (the system library)
- busybox (the system binaries)

There are input and output provided in two components:
- toolchain (the compiler sdk)
- rootfs (the root filesystem)

```
61M     x86_64-buildroot-linux-uclibc_sdk-buildroot.tar.gz
756K    x86_64-rootfs.tar.gz
61M     aarch64-buildroot-linux-uclibc_sdk-buildroot.tar.gz
760K    aarch64-rootfs.tar.gz
```

As usual with linux containers, the kernel is **not** included.
There is also no need for a bootloader, in the system image.

There is an `init` system available, but normally not used.
It is being provided by busybox, and based on shell scripts.


A container image is provided by importing the rootfs archive:

```Dockerfile
FROM scratch
ADD rootfs.tar /
CMD ["/bin/sh"]
```

The software packages to include are defined in a Makefile:

<https://buildroot.org/downloads/manual/manual.html#adding-packages>

There are host compilers for most of the available languages.
Such as C/C++, Go, Python, and so on.

This includes support for their language package dependencies.
Such as autotools, CMake, Go modules, etc.

----

Building all the required software is done using [Buildroot](https://buildroot.org/).

The default container image, with [Busybox](https://busybox.net/), is around 1.5 MB.

See [https://github.com/afbjorklund/buildroot4minimalism](https://github.com/afbjorklund/buildroot4minimalism)

Written by Anders F Björklund <https://github.com/afbjorklund>

See also: <https://en.wikipedia.org/wiki/Ma_(negative_space)>
