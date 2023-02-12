# Godot Builds 🏗️

Repository that periodically builds Godot. Supports mono, all platforms, and export templates.

This serves as a template for others to fork and easily automate building their own custom versions of Godot.

- [godot](https://github.com/godotengine/godot)

## Usage

1. Fork this repository
2. Edit [.github/actions/godot-download/action.yml](.github/actions/godot-download/action.yml) to download your modules into the `modules` folder. See the example below for reference:

```yaml
name: Download Godot source code
description: Download and extract the Godot source code for use in compiling
inputs:
  version:
    description: Version of Godot source code to download
    default: "3.5-stable"
runs:
  using: "composite"
  steps:
    - name: Download and extract Godot
      shell: bash
      run: |
        curl -fLO https://github.com/godotengine/godot/archive/${{ inputs.version }}.tar.gz
        tar -xvzf ${{ inputs.version }}.tar.gz --strip-components 1 --exclude=".github"
    
		# Custom steps to download and moving sg-physics to the modules folder. 
    - name: Download sg-physics
      shell: bash
      run: |
        git clone https://gitlab.com/Atlinx/sg-physics-2d
        cd sg-physics-2d
    
    - name: Move sg-physics-2d to modules folder
      shell: bash
      run: mv sg-physics-2d/godot/modules/sg_physics_2d modules
```

3. Push your changes to start the first build

	This repository will build whenever new changes are pushed, as well as every month. The builds will be  accessible from the release page of the repository.