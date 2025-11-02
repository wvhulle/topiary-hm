# Topiary Home Manager Module

A generic [Home Manager](https://github.com/nix-community/home-manager) module for [Topiary](https://github.com/tweag/topiary) formatter.

## Installation

```nix
{ pkgs, ... }:
let
  topiary-module = pkgs.fetchFromGitHub {
    owner = "blindfs";
    repo = "topiary-nushell";
    rev = "main";
    sha256 = "sha256-AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=";
  };
in
{
  imports = [ "${topiary-module}/default.nix" ];

  programs.topiary = {
    enable = true;
    languages.nu = {
      extensions = [ "nu" ];
      queryFile = "${topiary-module}/languages/nu.scm";
      grammar.source.git = {
        git = "https://github.com/nushell/tree-sitter-nu.git";
        rev = "18b7f951e0c511f854685dfcc9f6a34981101dd6";
      };
    };
  };
}
```

## Configuration

### Multiple Languages
```nix
programs.topiary = {
  enable = true;
  languages = {
    nu = {
      extensions = [ "nu" ];
      queryFile = ./nu.scm;
      grammar.source.git = {
        git = "https://github.com/nushell/tree-sitter-nu.git";
        rev = "18b7f951e0c511f854685dfcc9f6a34981101dd6";
      };
    };
    nickel = {
      extensions = [ "ncl" ];
      queryFile = ./nickel.scm;
      grammar.source.git = {
        git = "https://github.com/nickel-lang/tree-sitter-nickel";
        rev = "some-revision-hash";
      };
    };
  };
};
```

## Usage

```bash
topiary format script.nu
cat script.nu | topiary format --language nu
```

The module automatically:
- Installs topiary package
- Generates `languages.ncl` configuration from your language definitions
- Sets `TOPIARY_CONFIG_FILE` and `TOPIARY_LANGUAGE_DIR` environment variables for VS Code compatibility