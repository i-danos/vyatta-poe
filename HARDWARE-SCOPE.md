# HARDWARE-SCOPE: vyatta-poe

**Status: partly IN THE IMAGE. Not verified. Not supported.**

Hardware-vendor support is out of scope for this project. See the "Hardware support scope" section of the
[`build-iso` README](https://github.com/i-danos/build-iso#hardware-support-scope). Unlike the other hardware directories, some
packages built from this source **are installed in the image**. They are not removed here; this note only states what they are. Last commit here: 41d4506 2020-10-06.

## What this is

- `vyatta-poe`: Vyatta PoE integration
- `vyatta-poe-v1-yang`: vyatta-poe-v1 module
- `vyatta-interfaces-dataplane-poe-v1-yang`: vyatta-interfaces-dataplane-poe-v1 module

## Measured (image `i-danos_2608_20261030T0752-amd64.hybrid`, sha256 `5d9e25d937fc6dccb8df93bdbdb6a2bc1aa8dd8239491e77d2152f0c0c386fb4`, on 2026-11-02)

| binary package | in the image (SBOM, 1524 pkgs) | named in a package list | built by OBS (repo-index) |
|---|---|---|---|
| `vyatta-poe` | yes | no | yes |
| `vyatta-poe-v1-yang` | yes | `vyatta-poe.list.chroot` | yes |
| `vyatta-interfaces-dataplane-poe-v1-yang` | yes | `vyatta-poe.list.chroot` | yes |

In the image: `vyatta-poe`, `vyatta-poe-v1-yang`, `vyatta-interfaces-dataplane-poe-v1-yang`. Not in the image: none.

## Why they are in the image

`vyatta-poe-v1-yang` and `vyatta-interfaces-dataplane-poe-v1-yang` are named in `build-iso/config/package-lists/vyatta-poe.list.chroot`; the
`vyatta-poe` package is installed as a dependency of those two. No PoE hardware has been tested by this project, so the YANG models and the
package are present but **not verified and not supported**.

## How to verify this

From the workstation checkout root (`/home/aikon/danos`), with `NAMES="vyatta-poe vyatta-poe-v1-yang vyatta-interfaces-dataplane-poe-v1-yang"`:

```
# in the image? (an empty list means no)
python3 -c "import json,sys;n=set(sys.argv[1:]);S=json.load(open('releases/i-danos_2608_20261030T0752-amd64.hybrid/sbom.json'))['packages'];print(sorted(p['name'] for p in S if p['name'] in n))" $NAMES
# named in a package list? (no output means no)
for n in $NAMES; do grep -nE "^[[:space:]]*$n[[:space:]]*(#.*)?\$" build-iso/danos-sources/build-iso/config/package-lists/*.chroot; done
# built by OBS? (1 means yes)
for n in $NAMES; do echo $n $(grep -cE "^Package: $n\$" build-iso/danos-build/repo-index/Packages); done
```
