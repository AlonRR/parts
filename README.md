# parts

Small printed parts, each modelled in OpenSCAD and checked before it is printed. Each one is a folder under
`models/`, with its own README: what it is for, how it is measured, and how to print it.

| Part | |
|---|---|
| [USB-C panel plate](models/usbc-panel-plate/) | covers an oversized opening in a PC case's front panel round a panel-mount USB-C socket, the port's opening bridged without support |

## Getting the files

The parts use [scad-tools](https://github.com/AlonRR/scad-tools), a git submodule. Clone with it:
`git clone --recurse-submodules`, or `git submodule update --init` after a plain clone.
A ZIP from GitHub comes without it: from the unpacked folder, fetch the version this repository is pinned to.
In PowerShell:

```powershell
$pin = (Invoke-RestMethod https://api.github.com/repos/AlonRR/parts/contents/scad-tools).sha
Invoke-WebRequest "https://github.com/AlonRR/scad-tools/archive/$pin.zip" -OutFile scad-tools.zip
Remove-Item -Recurse -ErrorAction SilentlyContinue scad-tools
Expand-Archive scad-tools.zip . ; Rename-Item "scad-tools-$pin" scad-tools ; Remove-Item scad-tools.zip
```

or in a POSIX shell:

```sh
pin=$(curl -s https://api.github.com/repos/AlonRR/parts/contents/scad-tools | sed -n 's/.*"sha": *"\([0-9a-f]*\)".*/\1/p')
rm -rf scad-tools && mkdir scad-tools && curl -sL "https://github.com/AlonRR/scad-tools/archive/$pin.tar.gz" | tar xz --strip-components=1 -C scad-tools
```

## Checking a part

From the repository's root, with [OpenSCAD 2021.01](https://openscad.org/downloads.html), PrusaSlicer and
[uv](https://docs.astral.sh/uv/):

```sh
sh scad-tools/scripts/scad-check.sh models/usbc-panel-plate/usbc-panel-plate.scad
uv run scad-tools/scripts/printables.py
```

`scad-check.sh` renders the part, checks it is one closed solid, slices it and compares the model's `fdm_*`
values with the print profile's. `printables.py` writes each part in [`scad-project.toml`](scad-project.toml) as
one self-contained `.scad` with its STL, for Printables. The print profiles and the design rules the parts follow
are in the [3d-printing toolkit](https://github.com/AlonRR/3d-printing-toolkit).

## How a part arrives

A part comes here, with its history, once it is finished: from the
[3d-printing toolkit](https://github.com/AlonRR/3d-printing-toolkit), where the USB-C plate began, or from the
ideas kept elsewhere until they are ready. A part that grows electronics or firmware gets a repository of its own
instead. This repository's history only grows; it is never rewritten.

## Licence

The parts and their figures are [CC-BY-4.0](LICENSES/CC-BY-4.0.txt). Each file states its own licence, by an SPDX
header or in [`REUSE.toml`](REUSE.toml), following the [REUSE](https://reuse.software/) specification. See
[`LICENSE`](LICENSE).

_Parts of this repository were drafted with the help of an LLM agent; reviewed and verified locally._
