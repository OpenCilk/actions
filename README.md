# OpenCilk GitHub actions

These actions build different OpenCilk components from source, so that other workflows can incorporate the actions as steps in their own workflows.

## Usage: `build-opencilk-project`

The `build-opencilk-project` action checks out and builds the OpenCilk compiler.

```yaml
- uses: OpenCilk/actions/build-opencilk-project@main
  with:
    # OpenCilk-compiler subprojects to build.
    # Default: 'clang'
    projects: ''

    # Ninja build target.
    # Default: 'all'
    build_target: ''

    # List of OS's to build for.
    os_list: ''

    # Extra arguments to pass to CMake.  See https://llvm.org/docs/CMake.html
    # for an overview of LLVM-specific CMake options.
    extra_cmake_args: ''
```

This action outputs the installation directory for opencilk-project into the variable `opencilk-installdir` and the build directory for opencilk-project into the variable `opencilk-builddir`.

# Usage: `build-cheetah`

The `build-cheetah` action checks out and builds the OpenCilk runtime system.

```yaml
- uses: OpenCilk/actions/build-cheetah@main
  with:
    # Path to the opencilk-project installation directory.
    # Default: '$(pwd)/opencilk/install
    opencilk_install: ''

    # Path to the opencilk-project build directory.
    # Default: '$(pwd)/opencilk/build
    opencilk_build: ''

    # Ninja build target.
    # Default: 'install'
    build_target: ''

    # List of OS's to build for.
    os_list: ''

    # Extra arguments to pass to CMake.
    extra_cmake_args: ''
```

This action installs the OpenCilk runtime library into the `opencilk_install` directory, and it outputs the build directory for cheetah into the variable `cheetah-builddir`.

# Usage: `build-cilktools`

The `build-cilktools` action checks out and builds the OpenCilk runtime system.

```yaml
- uses: OpenCilk/actions/build-cilktools@main
  with:
    # Path to the opencilk-project installation directory.
    # Default: '$(pwd)/opencilk/install
    opencilk_install: ''

    # Path to the opencilk-project build directory.
    # Default: '$(pwd)/opencilk/build
    opencilk_build: ''

    # Ninja build target.
    # Default: 'install'
    build_target: ''

    # List of OS's to build for.
    os_list: ''

    # Extra arguments to pass to CMake.
    extra_cmake_args: ''
```

This action installs the cilktools libraries into the `opencilk_install` directory, and it outputs the build directory for cheetah into the variable `cilktools-builddir`.

## Example: Build opencilk-project and print its Clang version

```yaml
- name: Setup OpenCilk compiler
  id: build-opencilk
  uses: OpenCilk/actions/build-opencilk-project@main
  with:
    projects: clang
- name: Print clang version
  run: ${{ steps.build-opencilk.outputs.opencilk-installdir }}/bin/clang --version
```

## Example: Build opencilk-project and use it to buiild cheetah

```yaml
- name: Setup OpenCilk compiler
  id: build-opencilk
  uses: OpenCilk/actions/build-opencilk-project@main
  with:
    projects: clang
- name: Build cheetah
  id: build-cheetah
  uses: OpenCilk/actions/build-cheetah@main
  with:
    opencilk_install: ${{ steps.build-opencilk.outputs.opencilk-installdir }}
    opencilk_build: ${{ steps.build-opencilk.outputs.opencilk-builddir }}
```

## Example: Build opencilk-project, cheetah, and cilktools

```yaml
- name: Setup OpenCilk compiler
  id: build-opencilk
  uses: OpenCilk/actions/build-opencilk-project@main
  with:
    projects: clang
- name: Build cheetah
  id: build-cheetah
  uses: OpenCilk/actions/build-cheetah@main
  with:
    opencilk_install: ${{ steps.build-opencilk.outputs.opencilk-installdir }}
    opencilk_build: ${{ steps.build-opencilk.outputs.opencilk-builddir }}
- name: Build cilktools
  id: build-cilktools
  uses: OpenCilk/actions/build-cilktools@main
  with:
    opencilk_install: ${{ steps.build-opencilk.outputs.opencilk-installdir }}
    opencilk_build: ${{ steps.build-opencilk.outputs.opencilk-builddir }}
```

After these actions, the directory `${{ steps.build-opencilk.outputs.opencilk-installdir }}` will contain a complete installation of OpenCilk, including the compiler, runtime-system library, and Cilk tools.
