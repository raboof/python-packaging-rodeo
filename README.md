
# The Python Package Management Rodeo

![1600px-Boddington_Rodeo_2015_(128247531)](https://user-images.githubusercontent.com/1386642/113493567-22cf9700-9495-11eb-9780-27bdb5fb7333.jpeg)
Glenn Stampalia, CC BY 3.0 <https://creativecommons.org/licenses/by/3.0>, via Wikimedia Commons

## Introduction

Few tools can elegantly manage the following packages:
- tensorflow - is bloated and pins specific versions of many of  its dependencies
- cartopy - has many system dependencies
-  `<random pypi package>`. This can be easily installed by `pip install`, but isn't in most distributions. The tool docrep is a good example.

This situation usually requires using multiple package managers like this:

    apt-get install python3-cartopy python3-pip
    pip install tensorflow <random pypi package>
    
However, using multiple package managers leads to ugly/non-portable installation
scripts that are hard to maintain and not very composable. Moreover, it is hard
to lock dependencies, manage license, etc, across multiple tools.

This Rodeo is designed to find ONE tool that can manage these three dependencies
in an elegant way. To manage a dependency a tool must
1. be able to install a working  environment 
   1. in a reasonable amount of time
2. be able to lock this installed environment so that it can reproduced at a future date.

This project tests point (1)  using continuous integration (CI).

## Package Managers Compared

1. [anaconda](https://docs.conda.io/en/latest/)
2. [pip](https://pip.pypa.io/en/stable/)
3. apt-get (on an ubuntu system)
4. [Nix](https://nixos.org/)
   1. nixpkgs
   1. [poetry2nix](https://github.com/nix-community/poetry2nix)
   1. [mach-nix](https://github.com/DavHau/mach-nix)

The goal is to compare these on Mac and Linux, but only Linux is currently implemented.

## Results

The more packages a tool can install the better. Passing CI tests is good.

| Package Manager | CI Status   |  Packages Attempted to install [^1]  | 
|--------------------------|----------------|-------------------------------------------|
| pip | [![Python package](https://github.com/nbren12/python-packaging-rodeo/actions/workflows/pip.yaml/badge.svg?branch=master)](https://github.com/nbren12/python-packaging-rodeo/actions/workflows/pip.yaml) | tensorflow, docrep  |
| conda | [![Conda](https://github.com/nbren12/python-packaging-rodeo/actions/workflows/conda.yaml/badge.svg)](https://github.com/nbren12/python-packaging-rodeo/actions/workflows/conda.yaml) | cartopy, tensorflow  |
| nixpkgs |[![Nix (nixpkgs)](https://github.com/nbren12/python-packaging-rodeo/actions/workflows/nix.yaml/badge.svg)](https://github.com/nbren12/python-packaging-rodeo/actions/workflows/nix.yaml)| cartopy, tensorflow  |
| nixpkgs (machnix) | [![Nix (machnix)](https://github.com/nbren12/python-packaging-rodeo/actions/workflows/mach-nix.yaml/badge.svg)](https://github.com/nbren12/python-packaging-rodeo/actions/workflows/mach-nix.yaml) | cartopy, tensorflow, docrep |
| nixpkgs (poetry2nix) | [![Nix (poetry2nix)](https://github.com/nbren12/python-packaging-rodeo/actions/workflows/poetry2nix.yaml/badge.svg)](https://github.com/nbren12/python-packaging-rodeo/actions/workflows/poetry2nix.yaml) | cartopy, tensorflow, docrep |
 | apt-get  | [![Nix (machnix)](https://github.com/nbren12/python-packaging-rodeo/actions/workflows/apt-get.yaml/badge.svg)](https://github.com/nbren12/python-packaging-rodeo/actions/workflows/apt-get.yaml) | cartopy |

[^1]: Readers should assume that the tool CANNOT install more than this

## Contributing

Please contribute a new packaging tool or modify an existing tool's configuration to improve its results. That said, we will need to discuss what a "tools configuration" consists of. Some rough thoughts are:
- conda can install pip dependencies, but to my knowledge it does not ensure that pip versions installed are consistent with the resolved conda versions, and cannot be frozen very easily
- Nix-based tools have more leeway in this regard since its "configuration" is the nix programming language
- Manually packaging the `<random pip package>` (e.g. writing an explicit nix derivation, conda recipe, etc) is not allowed. 
