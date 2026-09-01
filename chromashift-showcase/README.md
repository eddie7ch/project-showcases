# ChromaShift: Color Accessibility Overlay

**Try it free (browser):** https://mlebotics.com/apps/chromashift-browser
**Buy the Windows app:** via [mlebotics.com/projects](https://mlebotics.com/projects)

A system-wide color-vision-deficiency (CVD) overlay for Windows that remaps
colors across the entire screen in real time — any app, browser, game, or
embedded site — so people with color blindness can distinguish colors
without per-app configuration.

This repository documents the project. The application source is closed;
see [Why no source code?](#why-no-source-code) below.

## Overview

ChromaShift is a desktop accessibility tool built to solve a concrete,
common problem: standard UIs routinely use color as the *only* way to
distinguish states (red/green status, error/success, chart series), which
fails for the ~8% of men with some form of color vision deficiency.
ChromaShift corrects for that at the OS level instead of per-application.

## Features

- **7 CVD types supported:** Deuteranopia, Deuteranomaly, Protanopia,
  Protanomaly, Tritanopia, Tritanomaly, Achromatopsia
- **3 correction modes:**
  - **Correct** — Daltonize algorithm, shifts confused colors so they're
    distinguishable
  - **High Contrast** — stronger 1.5× daltonization for maximum separation
  - **Simulate** — shows what content looks like to a CVD viewer (useful
    for designers/developers testing their own UI's accessibility)
- Intensity slider, live color preview, global hotkey toggle, system tray
  control, auto-start with Windows, persistent settings
- Free in-browser tab-filter version, or a paid system-wide Windows app

## How it works

Uses the Windows Magnification API (`MagSetFullscreenColorEffect`) — the
same API Windows' own built-in Magnifier accessibility tool uses — to apply
a 5×5 color transformation matrix to every pixel on screen. No screen
capture, no drivers, no admin rights required.

The correction itself implements the Daltonize algorithm (Fidaner et al.,
2008): a simulation matrix models what a CVD viewer perceives, then an
error-shift redistributes the color information a standard display loses
for that viewer into channels they can still perceive.

## Tech stack

- **Core:** Python, Windows Magnification API (via ctypes)
- **UI:** CustomTkinter (dark-theme desktop UI)
- **Packaging:** PyInstaller (standalone Windows .exe, no Python required)
- **Tray/hotkey:** pystray + Pillow, `keyboard`, `winreg` (Windows startup)

## Why no source code?

ChromaShift is sold as a paid Windows app through mlebotics.com. The
source used to be public in the MLEbotics/MLEbotics monorepo, which meant
anyone could clone and build the paid app themselves — so it was split out
into its own private repository. This showcase exists so the product can
still be evaluated (via the free browser trial and the case study) without
exposing source others could build from for free.

## Status

Built and shipped, sold live via mlebotics.com as of 2026.
