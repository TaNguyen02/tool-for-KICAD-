# Corresponding source code

KiCad Part Finder 2.0.1 is provided under AGPL-3.0-or-later. Application source, tests and build instructions accompany the executable in `source/`. The unmodified easyeda2kicad 1.0.1 source is in `source/vendor/easyeda2kicad/`. Offline interactive viewer assets and their upstream notices are in `source/assets/viewer3d/`.

The executable includes unmodified PyMuPDF 1.26.7 and MuPDF 1.26.12 under AGPL-3.0. The Python wrapper source and package metadata are also in `source/vendor/`. Complete upstream source, including native build sources and instructions, is available without charge:

- [PyMuPDF 1.26.7 source archive](https://files.pythonhosted.org/packages/48/d6/09b28f027b510838559f7748807192149c419b30cb90e6d5f0cf916dc9dc/pymupdf-1.26.7.tar.gz), 84,327,033 bytes.
- Archive SHA-256: `71add8bdc8eb1aaa207c69a13400693f06ad9b927bea976f5d5ab9df0bb489c3`.
- [PyMuPDF source tag 1.26.7](https://github.com/pymupdf/PyMuPDF/tree/1.26.7).
- [MuPDF source tag 1.26.12](https://github.com/ArtifexSoftware/mupdf/tree/1.26.12); clone recursively to obtain source submodules.
- [Upstream build instructions](https://pymupdf.readthedocs.io/en/latest/installation.html#build-and-install-from-source).

These source-access directions accompany the object code under AGPL section 6(d). `LICENSES/PYMUPDF-MUPDF-AGPL-3.0.txt` contains the license. Components retain their upstream copyright and third-party notices. This application does not modify these native libraries.

## Optional external AI components

Ollama and model weights are **not bundled** with the application. Installation and model download are separate user actions. The application's local-only HTTP client, prompts, schema and evidence checks are included in `source/`.

- [Ollama source](https://github.com/ollama/ollama), under the [MIT license](https://github.com/ollama/ollama/blob/main/LICENSE).
- Default model: [qwen3-vl:2b on Ollama](https://ollama.com/library/qwen3-vl:2b).
- [Qwen3-VL source and model links](https://github.com/QwenLM/Qwen3-VL), with the upstream [Apache-2.0 license](https://github.com/QwenLM/Qwen3-VL/blob/main/LICENSE).

The application does not require hosted inference or a paid API key. See `THIRD_PARTY_NOTICES.md` for the remaining runtime and packaging notices.

## KiCad library refresh integration

Version 2.0.1 keeps direct CAD-file and library-table imports while limiting automatic refresh to supported library browsers that are already open. It sends no menu command to open editors or browsers and schedules no automatic retry loop. A saved library and a browser whose new entries have been verified are reported separately.

The following upstream KiCad 10.0.6 sources explain how reopening a chooser can read updates to an already-registered native library. They are implementation references, not evidence of GUI verification for this patch:

- [Symbol chooser construction](https://gitlab.com/kicad/code/kicad/-/raw/10.0.6/eeschema/widgets/panel_symbol_chooser.cpp) and [symbol tree population](https://gitlab.com/kicad/code/kicad/-/raw/10.0.6/eeschema/symbol_tree_model_adapter.cpp).
- [Native symbol plugin](https://gitlab.com/kicad/code/kicad/-/raw/10.0.6/eeschema/sch_io/kicad_sexpr/sch_io_kicad_sexpr.cpp): `cacheLib` checks whether the library file changed before enumeration.
- [Footprint chooser construction](https://gitlab.com/kicad/code/kicad/-/raw/10.0.6/pcbnew/widgets/panel_footprint_chooser.cpp) calls `RefreshChangedLibraries` before populating the tree; the [footprint adapter](https://gitlab.com/kicad/code/kicad/-/raw/10.0.6/pcbnew/footprint_library_adapter.cpp) compares timestamps and re-enumerates changed libraries.

A chooser already open during an import must be closed and reopened. New library registration or a storage-path change may require accepting the row once in KiCad's library manager to update its current session. This integration does not enable the IPC API or change KiCad's security preferences.
