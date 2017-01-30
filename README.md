# ubuntu-kernel-build

A Docker container for building an Ubuntu kernel with arbitrary patches, config flags, and a tag.

## Example

A simple kernel build can be run like so:

```
sudo install --directory -o 1000 -g 1000 ~/kbuild
sudo docker run -it --rm -v ~/kbuild:/data \
    naftulikay/ubuntu-kernel-build
```

The end result will be a series of Debian kernel packages in the `~/kbuild` directory.

A more advanced build with patches and building `lowlatency`, `cloud`, and `generic` kernel packages:

```
sudo docker run -it --rm -v ~/kbuild:/data -v ~/patches:/patches \
    -e BUILD_TYPE=full naftulikay/ubuntu-kernel-build
```

## Environment Variables

### `KERNEL_MAJOR`

The major kernel version to build, by default `4.4.0`.

### `BUILD_TAG`

The tag appended to the package version to make the package unique, defaults to `hardcore` for obvious reasons.

### `BUILD_TYPE`

The type of kernel build to run, either `fast` (default), `full`, or `custom`.

If `custom`, `BUILD_CMD` must be specified.

### `BUILD_CMD`

If `BUILD_TYPE` is set to `custom`, this must be specified as arguments to `fakeroot debian/rules`.

## Volumes

### `/data`

This will be the directory where the kernel is built. It should be owned by a user with a UID of 1000.

### `/patches`

This is a directory containing patch files to apply against the kernel sources before building.
