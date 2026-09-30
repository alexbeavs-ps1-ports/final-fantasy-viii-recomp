<p align="center"><a href="https://alexbeavs-ps1-ports.github.io/psxrecomp-ports/"><img src="https://raw.githubusercontent.com/alexbeavs-ps1-ports/psxrecomp-ports/main/docs/assets/alexbeav-ps1-recomps-banner.png" alt="Alexbeav's PS1 Recomps" width="100%"></a></p>

# Final Fantasy VIII Recompiled

This release candidate uses PSXRecomp and the shared recomp-ui launcher.
You must supply your own SLUS-00892 game disc and SCPH-5502/5552 Europe BIOS.
The package contains no game disc, retail BIOS, generated retail game code, or saved game.

## Setup

1. Extract the complete setup ZIP into a writable folder.
2. Start `Final_Fantasy_VIII_Recompiled` (`.exe` on Windows).
3. Select your CUE file and the required BIOS in the setup wizard.
4. Run Generate & rebuild and wait for the game to start.

On Windows, the setup wizard can download the portable build tools.
On Linux and macOS, install CMake, Ninja, Python 3, and a C/C++ compiler first.
Keep the CUE and all files it references together.
The BIOS must be 524288 bytes with SHA-256 `1faaa18fa820a0225e488d9f086296b8e6c46df739666093987ff7d8fd352c09`.

## Candidate status

Version 0.1.0 is being prepared for September 7, 2026.
Package setup and gameplay acceptance are pending; native build checks do not establish gameplay support.
The exact source and dependency identities are in [project-manifest.toml](project-manifest.toml).
See [feasibility and validation](docs/FEASIBILITY.md) for the tested scope.

## About this project

This project was developed with AI assistance.
AI assists with code, documentation, and investigation; Alex tests the game and makes release decisions.
Validation claims describe only the tests actually performed.

## Credits and licenses

Framework: [PSXRecomp](https://github.com/mstan/psxrecomp).
Launcher: [recomp-ui](https://github.com/mstan/recomp-ui).
Their licenses and dependency notices remain in the corresponding source directories.
The original game and its trademarks belong to their respective owners.
