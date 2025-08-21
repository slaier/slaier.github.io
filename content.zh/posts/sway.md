---
title: "Sway on NixOS"
date: 2022-07-16T12:00:00+08:00
keywords: ["sway", "nixos"]
tags: ["sway", "nixos"]
pin: true
bookFlatSection: true
---

>Sway 是Wayland的合成器，也是 x11 的 i3 窗口管理器的替代品。它可以与您现有的 i3 配置一起工作，并支持 i3 的大部分特性以及一些附加功能。

<!--more-->

## 安装

NixOS 提供了 sway 的 Module, 直接 enable 即可。

```nix
programs.sway = {
  enable = true;
  wrapperFeatures.gtk = true;
};
```

## 启动

sway 可以通过 login shell 启动，也可以通过 Display Manager 启动。

通过 Display Manager 启动需要使用 GDM Wayland 和 xserver，我因为同时安装了 i3，所以我选择使用 Display Manager 启动。

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

## 提示和技巧

### 使用 rofi 启动应用程序

```nix
''
${pkgs.rofi-wayland}/bin/rofi -show drun
''
```

### 切换窗口

rofi-wayland 没有直接集成此功能，使用swayr进行替代。

```nix
''
  bindsym ${mod}+Shift+d exec ${lib.getExe pkgs.swayr} switch-window
  bindsym ${mod}+Tab     exec ${lib.getExe pkgs.swayr} switch-to-urgent-or-lru-window
''
```

## 疑难解答

### Fcitx 不出现候选框

这是因为 sway 还没有实现 input-method popups。

可以打入该补丁：[0001-text_input-Implement-input-method-popups.patch](https://raw.githubusercontent.com/Slaier/nixos-profile/39f35767099c5083ee416909643a10fc9b0f9ca3/overlays/sway/0001-text_input-Implement-input-method-popups.patch)。

### Swaybar 的 tray icon 无法正常使用

将 swaybar 换成 waybar，swaybar 本身支持不好。

```nix
''
bar swaybar_command ${pkgs.waybar}/bin/waybar
''
```
