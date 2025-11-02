# Topiary Home Manager Module

A [Home Manager](https://github.com/nix-community/home-manager) module for [Topiary](https://github.com/tweag/topiary) formatter.

## Usage

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
      queryFile = ./nu.scm;
      grammar.source.git = {
        git = "https://github.com/nushell/tree-sitter-nu.git";
        rev = "18b7f951e0c511f854685dfcc9f6a34981101dd6";
      };
    };
  };
}
```

Automatically sets up topiary configuration and environment variables for VS Code compatibility.