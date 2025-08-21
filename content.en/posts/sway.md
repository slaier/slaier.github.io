---
title: "Sway on NixOS"
date: 2022-07-16T12:00:00+08:00
keywords: ["sway", "nixos"]
tags: ["sway", "nixos"]
pin: true
bookFlatSection: true
---

>Sway is a Wayland compositor and a drop-in replacement for the i3 window manager for X11. It works with your existing i3 configuration and supports most of i3's features, plus a few extras.

<!--more-->

## Installation

NixOS provides a module for sway, which can be enabled directly.

```nix
programs.sway = {
  enable = true;
  wrapperFeatures.gtk = true;
};
```

## Starting

Sway can be started from a login shell or from a Display Manager.

Starting from a Display Manager requires GDM Wayland and xserver. Since I also have i3 installed, I choose to start it from a Display Manager.

```nix
services.xserver = {
  enable = true;
  desktopManager = {
    xterm.enable = false;
  };
  displayManager = {
    defaultSession = "sway";
    gdm = {
      enable = true;
      wayland = true;
    };
  };
};
```

## Tips and Tricks

### Launching applications with rofi

```nix
''
${pkgs.rofi-wayland}/bin/rofi -show drun
''
```

### Switching windows

rofi-wayland does not have this feature integrated directly. Use swayr as a replacement.

```nix
''
  bindsym ${mod}+Shift+d exec ${lib.getExe pkgs.swayr} switch-window
  bindsym ${mod}+Tab     exec ${lib.getExe pkgs.swayr} switch-to-urgent-or-lru-window
''
```

## Troubleshooting

### Fcitx candidate box does not appear

This is because sway has not yet implemented input-method popups.

You can apply this patch: [0001-text_input-Implement-input-method-popups.patch](https://raw.githubusercontent.com/Slaier/nixos-profile/39f35767099c5083ee416909643a10fc9b0f9ca3/overlays/sway/0001-text_input-Implement-input-method-popups.patch).

### Swaybar's tray icon does not work properly

Replace swaybar with waybar. swaybar itself is not well supported.

```nix
''
bar swaybar_command ${pkgs.waybar}/bin/waybar
''
```