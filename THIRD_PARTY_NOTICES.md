# Third-party notices

KiCad Part Finder 4.1.0 is distributed under GNU AGPL version 3 or later. Copyright (c) 2026 KiCad Part Finder contributors. See `LICENSE` for the full license text.

## Freerouting 2.5.0

- [Project and contributors](https://github.com/freerouting/freerouting) and [official v2.5.0 release](https://github.com/freerouting/freerouting/releases/tag/v2.5.0).
- License: GNU GPL version 3. The unmodified official Windows x64 native CLI archive is included as a separate runtime asset; Java is not required to run it.
- The integration calls the external engine through native KiCad DSN/SES exchange. No Altium code is included, and the app does not install or require a KiCad autorouting plugin.
- License: `LICENSES/FREEROUTING-GPL-3.0.txt`; archive provenance/checksum: `source/assets/autorouter/SOURCE.md`. Corresponding upstream source and build instructions are linked in [SOURCE_CODE.md](SOURCE_CODE.md).
- Upstream runtime components keep their own included notices. Freerouting authorship remains with its contributors; this app does not claim authorship of its routing engine.

## easyeda2kicad 1.0.1

- Author: uPesy and contributors.
- [Source](https://github.com/uPesy/easyeda2kicad.py) and [versioned distribution](https://pypi.org/project/easyeda2kicad/1.0.1/).
- License: GNU AGPL version 3; original license included in `source/vendor/easyeda2kicad-LICENSE`.
- Unmodified Python package source is included in `source/vendor/easyeda2kicad/`. Application code uses this package for symbol and footprint conversion. Converter authorship remains with its upstream contributors.

## PyMuPDF 1.26.7 / MuPDF 1.26.12

- Copyright Artifex Software and contributors.
- License: GNU AGPL version 3; this distribution uses the AGPL option.
- Sources: [PyMuPDF](https://github.com/pymupdf/PyMuPDF) and [MuPDF](https://github.com/ArtifexSoftware/mupdf).
- Unmodified native libraries render PDF, SVG and image previews and provide local PDF text/table access. License included in `LICENSES/PYMUPDF-MUPDF-AGPL-3.0.txt`.
- Versioned complete-source download directions and archive checksum appear in `SOURCE_CODE.md` next to the executable. Python wrapper source is included in `source/vendor/pymupdf/`.

## Offline interactive 3D viewer

- [Three.js r180 (0.180.0)](https://github.com/mrdoob/three.js/tree/r180), Copyright Three.js Authors, MIT license.
- [Chevrotain 9.0.1](https://github.com/Chevrotain/chevrotain/tree/v9.0.1), Apache-2.0 license.
- [Embedded regexp-to-ast 0.5.0](https://github.com/bd82/regexp-to-ast/tree/v0.5.0), MIT license.
- Unmodified upstream JavaScript modules, license texts, attribution and file checksums are in `source/assets/viewer3d/`. Runtime assets are bundled locally; a prepared scene does not require a CDN.

## Optional local AI: separately installed

- [Ollama](https://github.com/ollama/ollama), Copyright Ollama, [MIT license](https://github.com/ollama/ollama/blob/main/LICENSE).
- Default optional model: [Qwen3-VL 2B / qwen3-vl:2b](https://ollama.com/library/qwen3-vl:2b), [Apache-2.0 license](https://github.com/QwenLM/Qwen3-VL/blob/main/LICENSE).
- Ollama binaries and model weights are not included in the release. Users obtain them separately from their upstream sources. Other selected models retain their own licenses.
- The integration calls a local Ollama endpoint, uses structured extraction, and checks source evidence before CAD generation. It does not provide a hosted AI account, paid API, or cloud-document service.

## Python 3.12 and Tcl/Tk

The executable includes the Python runtime and Tcl/Tk supplied by the build runtime. Python's license and component notices are in `LICENSES/PYTHON-LICENSE.txt`; Tcl/Tk notices are in `LICENSES/TCL-license.txt` and `LICENSES/TK-license.txt` when supplied by the runtime. Runtime files retain their respective licenses.

## PyInstaller 6.22.3

[Project](https://pyinstaller.org/) and [source](https://github.com/pyinstaller/pyinstaller).

Build tool and bootloader, GPL version 2 or later with the bootloader exception. Original license and exception included in `LICENSES/PYINSTALLER-COPYING.txt`. PyInstaller is used for packaging; upstream build-tool source is available from the project above.

## Data sources and trademarks

KiCad, Freerouting, Altium, LCSC, EasyEDA, JLCPCB, Ollama, and Qwen are names of their respective projects/owners. This is an independent application, not an official product of those projects or organizations.

Downloaded CAD, datasheets, product metadata and 3D models retain their source licenses and terms. Program licensing does not automatically relicense downloaded component data. Public search providers and manufacturer sites are external services rather than bundled dependencies; endpoint availability and access requirements may change.

Reviewed pin-profile records include factual terminal mappings and references to documents authored by STMicroelectronics, Ai-Thinker, Texas Instruments and Advanced Monolithic Systems, plus KiCad ERC conventions. Source URLs, revisions and available file hashes are recorded in `source/assets/pin_profiles.json` and explained in `source/assets/PIN_PROFILES.md`. No manufacturer PDF or KiCad symbol artwork is bundled as part of those facts. Manufacturer names and component identifiers remain with their respective owners; the records do not imply endorsement or certify a design.
