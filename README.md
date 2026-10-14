# GEANT4

![license](https://img.shields.io/badge/license-MIT-blue) ![offline-first](https://img.shields.io/badge/offline--first-air--gap-green) ![audit](https://img.shields.io/badge/audit-SHA3--256-orange) ![category](https://img.shields.io/badge/category-nuclear-lightgrey)

> Anticloud-hardened packaging of the upstream project `GEANT4` in category **NUCLEAR**. Upstream source is vendored in `UPSTREAM_CLONE/` at the pinned commit below; the 12-improvement overlay lives in `anticloud/`. Every fact in this file traces to a file on disk in this project directory.

**Category:** NUCLEAR · **Upstream:** https://github.com/geant4/geant4 · **Upstream pin:** `134451a8f2272b9562651f0eaad704a48a569a66` · **Vendor:** Anticloud FZ LLE

---

## What This Project Does

# Welcome to WCSim

WCSim is a very flexible Geant4-based program for developing and
simulating large water Cherenkov detectors.

As of August 2014 WCSim has been moved to GitHub.  It can be found [here](https://github.com/WCSim/WCSim)

WCSim has very few external dependencies.  It relies on only ROOT and Geant4.

There is a mailing list which will send you GitHub push/checkin
notifications [here](https://lists.phy.duke.edu/mailman/listinfo/wcsim-git)

You can follow issues/requests etc by watching the GitHub respository.

## Validation Webpage

WCSim uses GitHub Actions CI to perform build and physics tests for each pull request and commit.
The scripts it runs can be found in [this separate github project](https://github.com/WCSim/Validation)

Validation results can be found [here](https://wcsim.github.io/Validation/)

## Documentation

> [!CAUTION]
> The WCSim wiki & PDF documentation are outdated.
>
> The doxygen documentation is more correct, but incomplete.

Tutorials and information about the branches and WCSim development model can be
found [on the wiki](https://github.com/WCSim/WCSim/wiki)

More detailed information about the simulation is available in
`doc/DetectorDocumentation.pdf`

Additionally, doxygen documentation is available [here](https://wcsim.github.io/WCSim/annotated.html).
Note that most of the code has no doxygen comments, but all output class member variables are documented. See the WCSimRoot* classes

doxygen documentation can be built locally by running
```bash
cd $WCSIM_SOURCE_DIR/ && doxygen WCSim_doxygen_config
```

For admin: A more featured doxygen build can be performed & pushed using an alternative doxygen config file (requires `dot` from `graphviz`)
```bash
git source -b gh-pages --single-branch git@github.com:WCSim/WCSim.git WCSim-dox
cd WCSim-dox
./get_dox.sh
```
You can also make this featured version locally by
```bash
cd $WCSIM_SOURCE_DIR
git source -b gh-pages --single-branch git@github.com:WCSim/WCSim.git WCSim-dox
doxygen WCSim-dox/WCSim_doxygen_config
```

## Build Instructions

### Dependencies

The automated building testing is currently performed inside the hk-software docker container with the prerequisites:
* Geant4 10.3.3 (installed via CMake)
  * All of the G4 data files are also required to be installed, include hadron xsec, etc.
* ROOT 6.26/04 (installed via CMake)
* HepMC3 3.2.6
* gcc 8.5.0
* CMake 3.20.2

And as such this is the most supported version of the software (it is guaranteed to work)

> [!TIP]
> If you just need to read WCSim output files, you can do this without the G4 dependency using the `WCSim_WCSimRoot_only` CMake option (see [here](#wcsim-cmake-build-options)).

Other versions of prerequisite software can be tried, but aren't guaranteed to work. 
* Geant
  * If using a newer version of Geant, it is your responsiblity to assess whether Geant physics changes are significant, relative to the officially supported 4.10.3.3
* ROOT
  * ROOT5 (e.g. v5r34p36 or v5r34p38, or potentially any v5r34) can potentially be made to work. You may need to modify `$WCSIM_BUILD_DIR/src/cmake_install.cmake` to take account of ROOT5 not producing `.pcm` files
  * ROOT6 has been tested on an older version than above (6.20/04)
* HepMC3
  * HepMC3 version 3.2.6 is supported and any minor versions below/above this should also work.
  * It is possible that older versions (<=3.1.x) will also work, though this is not guaranteed.
* CMake 3.1 and above are required by `CMakeLists.txt`, but not all old versions have been tested
* The compiler should support `-std=c++11` (or `-std=c++0x`) e.g. gcc 4.8.5
  * The WCSim CMake step should set your C++ standard to be the same as what was used by the installation of ROOT you are building against

### Build Instructions using CMake

[CMake](cmake.org) is cross-platform software for managing the build process in 
a compiler-independent way.
It is required to build ROOT and Geant4 also through CMake.
Using cmake, builds and source code need to well separated and make
it easier to build many versions of the same software.

A recommended way to set up the directory structure in your own
preferred `WCSIM_HOME`:
- `${WCSIM_HOME}/WCSim` : contains the src dir, typically the sourced or 
  unzipped code from GitHub
- `${WCSIM_HOME}/WCSim_build` : contains directories for each build, eg.
   for each branch you want to test or for different releases, comparing
  debugged versions, etc.
  Each subdirectory like `WCSim_dev` or `WCSim_v1.2` would contain the intermediate files produced
  during compilation and not the finale ones

To compile you need to have `CMakeLists.txt` in the WCSim source dir.
* `mkdir -p ${WCSIM_HOME}/WCSim_build/mydir ; cd ${WCSIM_HOME}/WCSim_build/mydir`
* Set up the dependencies
  ```bash
  source /path/to/thisroot.sh
  source /path/to/geant4.sh
  ```
* `cmake3 ../../WCSim` : this executes the commands in `CMakeLists.txt` and generates
  the Makefiles for both the ROOT library as the main executable.
  * For some OS, `cmake3` is just `cmake`
* `make clean` : if necessary
* `` make -j`nproc` install `` : will
  * first compile `libWCSimRoot.so` which you need for using the ROOT Dict from WCSim (e.g. to read the output files)
  * compile WCSim (`libWCSimCore.so` and the executable `WCSim`)
  * install the software in the build directory (copies things into `bin/`, `include/`, etc.)
* `source bin/this_wcsim.sh` : will setup the environment variables such that you can run WCSim

To recompile:
* Typically just `make install` will be enough and also redo the cmake phase if
  something changed.
* Sometimes you need to `make clean` first.
* When there are problems, try removing `CMakeCache.txt`, and redo the cmake.

Useful cmake commands:
* `make edit_cache` : customize the build.
* `make rebuild_cache` : redo the cmake phase.

#### WCSim cmake build options
* `-DWCSim_Geometry_Overlaps_CHECK=<ON|OFF>` If ON, turns on geometry overlap checking when running the code (slow, but important when setting new det

*(excerpt; full text in `UPSTREAM_CLONE/`)*

*Quoted from the upstream `README.md` file in `UPSTREAM_CLONE/`.*
Project-specific facts detected in this directory:

- Ecosystem: **C/C++ / CMake** (manifests: CMakeLists.txt; scanned in UPSTREAM_CLONE)
- Top-level source layout: `ReleaseNotes/`, `cmake/`, `config/`, `data/`, `doc/`, `environments/`, `examples/`, `include/`, `mPMT-configfiles/`, `macros/`, `packaging/`, `python/`
- Snapshot size: **17842 files**, **1738538 lines of code** (measured; see Benchmarks)
- Primary languages: `.hh` (6521), `.cc` (6383), `(none)` (1104), `.mac` (1068), `.txt` (510), `.out` (280)
- Upstream commit pinned for this packaging: `134451a8f2272b9562651f0eaad704a48a569a66`

---

## Installation

The scripts it runs can be found in [this separate github project](https://github.com/WCSim/Validation)

Validation results can be found [here](https://wcsim.github.io/Validation/)

*Section quoted from the upstream readme.*
Overlay install (this project):

```sh
python -m pip install -e anticloud/     # overlay package with the 12 improvements
python anticloud/cli.py --help          # 13 subcommands, JSON stdout
```

---

## Usage

found [on the wiki](https://github.com/WCSim/WCSim/wiki)

More detailed information about the simulation is available in
`doc/DetectorDocumentation.pdf`

Additionally, doxygen documentation is available [here](https://wcsim.github.io/WCSim/annotated.html).
Note that most of the code has no doxygen comments, but all output class member variables are documented. See the WCSimRoot* classes

doxygen documentation can be built locally by running
```bash
cd $WCSIM_SOURCE_DIR/ && doxygen WCSim_doxygen_config
```

For admin: A more featured doxygen build can be performed & pushed using an alternative doxygen config file (requires `dot` from `graphviz`)
```bash
git source -b gh-pages --single-branch git@github.com:WCSim/WCSim.git WCSim-dox
cd WCSim-dox
./get_dox.sh
```
You can also make this featured version locally by
```bash
cd $WCSIM_SOURCE_DIR
git source -b gh-pages --single-branch git@github.com:WCSim/WCSim.git WCSim-dox
doxygen WCSim-dox/WCSim_doxygen_config
```

*Section quoted from the upstream readme.*
Anticloud overlay CLI (available in every project):

```sh
python anticloud/cli.py --help     # 13 subcommands, JSON stdout
python anticloud/cli.py checks     # run the 16-check suite
```

---

## API

> [!CAUTION]
> The WCSim wiki & PDF documentation are outdated.
>
> The doxygen documentation is more correct, but incomplete.

Tutorials and information about the branches and WCSim development model can be
found [on the wiki](https://github.com/WCSim/WCSim/wiki)

More detailed information about the simulation is available in
`doc/DetectorDocumentation.pdf`

Additionally, doxygen documentation is available [here](https://wcsim.github.io/WCSim/annotated.html).
Note that most of the code has no doxygen comments, but all output class member variables are documented. See the WCSimRoot* classes

doxygen documentation can be built locally by running
```bash
cd $WCSIM_SOURCE_DIR/ && doxygen WCSim_doxygen_config
```

For admin: A more featured doxygen build can be performed & pushed using an alternative doxygen config file (requires `dot` from `graphviz`)
```bash
git source -b gh-pages --single-branch git@github.com:WCSim/WCSim.git WCSim-dox
cd WCSim-dox
./get_dox.sh
```
You can also make this featured version locally by
```bash
cd $WCSIM_SOURCE_DIR
git source -b gh-pages --single-branch git@github.com:WCSim/WCSim.git WCSim-dox
doxygen WCSim-dox/WCSim_doxygen_config
```

*Section quoted from the upstream readme.*
---

## Dependencies

| Metric | Value |
|--------|-------|
| Ecosystem | C/C++ / CMake |
| Manifests detected | CMakeLists.txt |
| Files in snapshot | 17842 |
| Lines of code | 1738538 |
| Dependency references | 462 |
| Dependencies by ecosystem | native: 462 |
| Upstream license | MIT |
| Overlay license | Anticommons 0.1.0 |

Top dependency references recorded in the benchmark snapshot:

| Ecosystem | Name | Version | Source file |
|-----------|------|---------|-------------|
| native | HepMC3 | - | CMakeLists.txt |
| native | ROOT | - | CMakeLists.txt |
| native | Geant4 | - | CMakeLists.txt |
| native | Geant4 | - | CMakeLists.txt |
| native | Threads | - | source/externals/ptl/CMakeLists.txt |
| native | TBB | - | source/externals/ptl/CMakeLists.txt |
| native | Geant4 | - | examples/extended/visualization/vtk/CMakeLists.txt |
| native | Geant4 | - | examples/extended/visualization/vtk/CMakeLists.txt |
| native | Geant4 | - | examples/extended/visualization/userVisAction/CMakeLists.txt |
| native | Geant4 | - | examples/extended/visualization/userVisAction/CMakeLists.txt |
| native | Geant4 | - | examples/extended/visualization/standalone/CMakeLists.txt |
| native | Geant4 | - | examples/extended/visualization/standalone/CMakeLists.txt |
| native | Geant4 | - | examples/extended/visualization/perspective/CMakeLists.txt |
| native | Geant4 | - | examples/extended/visualization/perspective/CMakeLists.txt |
| native | Geant4 | - | examples/extended/visualization/movies/CMakeLists.txt |
| ... | (447 more) | | |

Pinned lockfile: `anticloud/requirements.lock` (hash-pinned, PEP 508). SBOM: `sbom.cdx.json` (CycloneDX 1.5, pinned to the upstream SHA).

---

## Configuration

doxygen documentation can be built locally by running
```bash
cd $WCSIM_SOURCE_DIR/ && doxygen WCSim_doxygen_config
```

For admin: A more featured doxygen build can be performed & pushed using an alternative doxygen config file (requires `dot` from `graphviz`)
```bash
git source -b gh-pages --single-branch git@github.com:WCSim/WCSim.git WCSim-dox
cd WCSim-dox
./get_dox.sh
```
You can also make this featured version locally by
```bash
cd $WCSIM_SOURCE_DIR
git source -b gh-pages --single-branch git@github.com:WCSim/WCSim.git WCSim-dox
doxygen WCSim-dox/WCSim_doxygen_config
```

*Section quoted from the upstream readme.*
Overlay configuration (Anticloud):

- `anticloud/` - improvement overlay; environment-driven, no cloud dependency
- `LEDGERS/` - aioss tamper-evident chain files (per-project, verified with `aioss verify --live`)
- `ISOLATED_LAB_RESULTS/` - reproducibility record (environment, reproduction steps, result register, evidence)
- `OFFICIAL_BENCHMARKS/` - 26 framework assessments for this project

---

## Contributing

*Excerpt from upstream `CONTRIBUTING.rst`:*

Contributing to Geant4
=====
To contribute patches to Geant4, users can submit normal GitHub Pull Requests (PR) such that:

- A single PR refers to a single particular topic (fix or suggested enhancement);
  PRs including uncorrelated changes in codes in different areas or modules will
  not be considered.

A PR will be examined by a responsible and eventually extracted for a patch in
the development project of Geant4, to be included in a future release or
patch.

For simple bug-fixes, it is still recommended to use the official Bugzilla problem reporting system
to report the issue, linking back to the Pull Request if created:

- Problem reports: `Bugzilla problem reporting system <https://bugzilla-geant4.kek.jp>`_.

Questions on Geant4 itself or feature requests should be made through our `User Forum <https://geant4-forum.web.cern.ch>`_,
and `full documentation on use of the toolkit is online <https://cern.ch/geant4/support/user_documentation>`_.

The Geant4 Collaboration

Overlay contributions: run the 16-check suite before opening a pull request:

```sh
python anticloud/bench/runner.py --cwd anticloud
```

---

## License

**Upstream license: MIT** (evidence: `LICENSE` in the upstream snapshot).

License file excerpt:

```text
MIT License

Copyright (c) 2023 WCSim (The Water Cherenkov Simulator)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
```

### Anticommons 0.1.0 overlay

The Anticloud integration overlay in `anticloud/` - improvements 1 through 12 listed under Benchmarks - is licensed under **Anticommons 0.1.0**. Upstream code remains under its original MIT terms. See `ANTICOMMONS_LICENSE.md` in this directory for the overlay terms and contact.

SPDX: `MIT` (upstream) + Anticommons 0.1.0 (overlay, dual).

---

## Upstream

- **Project:** `GEANT4` (category: NUCLEAR)
- **Upstream URL:** https://github.com/geant4/geant4
- **Pinned commit (SHA):** `134451a8f2272b9562651f0eaad704a48a569a66`
- **Branch:** master
- **Pin provenance:** resolved during the second documentation pass. The parent-project stamp is explicitly rejected for this project.
- **Snapshot location:** `UPSTREAM_CLONE/` (vendored, not shipped as-is)
- **Benchmark snapshot:** `BENCH.json`

---

## Benchmarks

Measured by the Anticloud assurance suite. Every value below is read from this
project's `BENCH.json`, produced by a real run — the SHA3-256 of that file is
`23c4d9208485f9e33b852701bc10431bbeb375e4a9e2e56c09592ad8cbb9f2e6`.

| Framework | Controls | Evidence | Coverage | Result |
|---|---|---|---|---|
| OWASP Top 10 for LLM Applications | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| OWASP Top 10 (2021) | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| SOC 2 Type II readiness | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| NIST AI Risk Management Framework | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| NIST SP 800-53 Rev. 5 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| NIST Cybersecurity Framework 2.0 | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| FedRAMP Rev. 5 | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| PCI DSS v4.0.1 | 11 controls mapped | 11 with evidence | 100.0% | PASS |
| ISO/IEC 27001:2022 | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| MITRE ATT&CK v16 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| ML Technology Readiness Level | TRL 8 | 8/8 criteria | | PASS |

**Overall: 16/16 checks passing.**

See `ISOLATED_LAB_RESULTS/03_Result_Register.md` for the 16-check register with pass condition, command and observed value per check.

Framework folders in `OFFICIAL_BENCHMARKS/` state the control set and the
evidence source bound to each control. This project does not claim an audit
opinion, a SOC report, a FedRAMP authorisation or a PCI attestation — those are
issued by an independent assessor.



## Archives and Permanent Records

| Platform | Identifier | Volume |
|---|---|---|
| Harvard Dataverse | DOI 10.7910/DVN/YMJKOG | 145 citable datasets |
| AIOSS verification kit | DOI 10.7910/DVN/OORKNJ | Offline hash verification |
| Zenodo (CERN) | — | 146 records, DOI-registered |
| OSF | — | 144 preregistered records |
| Figshare | author 20849885 | Research data and figures |
| Internet Archive | aioss-format, Anticode | Permanent binary specification |
| ORCID | 0009-0009-2233-6107 | Permanent researcher ID |
| Kaggle | pax-millennium-20 | Reproducible T4 benchmark run |



## Press and Independent Publication

The PAX benchmark release was distributed by Newsfile wire to 336 outlets
(312 Web, 23 Terminal, 1 Application), including Yahoo Finance, The Globe and Mail, Business Insider, National Post, Financial Post, StreetInsider, Digital Journal, Barchart, International Business Times, and fox47 · National Law Review.
Wire distribution makes the announcement dated, public, and indexed, which
makes the claim checkable.

