cmake_minimum_required(VERSION 3.20)
set(PSXRECOMP_ROOT "${CMAKE_CURRENT_SOURCE_DIR}/psxrecomp" CACHE PATH
    "Path to psxrecomp")
if(NOT EXISTS "${PSXRECOMP_ROOT}/runtime/runtime.cmake")
    message(FATAL_ERROR
        "PSXRecomp runtime is missing: ${PSXRECOMP_ROOT}/runtime/runtime.cmake\n"
        "This project expects the psxrecomp framework checkout at ${PSXRECOMP_ROOT}.\n"
        "Run: git submodule update --init --recursive\n"
        "If psxrecomp/ is not a submodule, set -DPSXRECOMP_ROOT=<path-to-psxrecomp>.")
endif()

project(Final_Fantasy_VIII_Recompiled LANGUAGES C CXX)

# --- Opt-in product surfaces (OFF by default in the framework) -------------
# Scaffold prompts / flags fill these. Without recomp-ui, PSX_RECOMP_UI is OFF.
# Setup-host CI requires WIZARD_CMAKE_BLOCK / ENABLE_SETUP_WIZARD (FORCE_SETUP_HOST
# alone does not open first-run / Generate & rebuild).
set(PSX_RECOMP_UI ON CACHE BOOL "Shared launcher" FORCE)
set(PSX_DEBUG_TOOLS OFF CACHE BOOL "Production build" FORCE)
set(PSX_STATIC_RUNTIME ON CACHE BOOL "Portable runtime" FORCE)
set(PSX_ENABLE_VULKAN ON CACHE BOOL "Vulkan renderer" FORCE)
set(PSX_NETPLAY OFF CACHE BOOL "Accepted offline release" FORCE)
set(PSX_SETUP_WIZARD ON CACHE BOOL "First-run setup" FORCE)
set(PSXRECOMP_BIOS_STEMS "SCPH5552" CACHE STRING "Accepted BIOS" FORCE)
set(PSXRECOMP_BIOS_STEM "SCPH5552" CACHE STRING "Accepted BIOS" FORCE)
set(PSXRECOMP_BIOS_PROFILE "${PSXRECOMP_ROOT}/bios/SCPH5552.toml" CACHE FILEPATH "Accepted BIOS" FORCE)
include("${PSXRECOMP_ROOT}/runtime/runtime.cmake")

psxrecomp_add_game_runtime(psx-runtime


    ENABLE_SETUP_WIZARD
    EXE_NAME "Final_Fantasy_VIII_Recompiled"
    WINDOW_TITLE "Final Fantasy VIII Recompiled"
    DEFAULT_GAME_CONFIG_PATH "game.toml"
    MAX_PLAYERS 1


    GEN_MARKER "generated/SLUS_008.92_dispatch.c"
    GEN_FULL_GLOB "generated/SLUS_008.92_full_*.c"
    CODEGEN_SETUP_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/codegen_setup.c"
)

# Stage runtime configs next to the exe for direct build/ launches. Only copy
# files that exist — never fail POST_BUILD when game_options.toml is absent.
foreach(_PSX_RUNTIME_CONFIG game.toml game_options.toml)
    if(EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/${_PSX_RUNTIME_CONFIG}")
        add_custom_command(TARGET psx-runtime POST_BUILD
            COMMAND ${CMAKE_COMMAND} -E copy_if_different
                "${CMAKE_CURRENT_SOURCE_DIR}/${_PSX_RUNTIME_CONFIG}"
                "$<TARGET_FILE_DIR:psx-runtime>/${_PSX_RUNTIME_CONFIG}"
            COMMENT "Staging ${_PSX_RUNTIME_CONFIG}"
            VERBATIM)
    endif()
endforeach()
