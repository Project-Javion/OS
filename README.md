# Project-Javion/OS — Starter aarch64 initramfs

This repository contains a very small, beginner-friendly Linux userspace (initramfs) you can boot and iterate on. It targets aarch64 (ARM64) so you can test easily on your Apple Silicon Mac (M1/M2/M4) using QEMU, and later deploy to ARM hardware (Raspberry Pi 4/5, UEFI aarch64 boards, or other ARM servers).

What I added
- init — a simple shell-based init script that runs as PID 1 inside the initramfs
- src/init.c — a minimal C init (fallback) that mounts /proc,/sys,/dev and drops to a shell if present
- Makefile — automates building a tiny rootfs using Docker (alpine) and packing initramfs; also provides a run-qemu target
- qemu-run.sh — convenience wrapper to run QEMU on macOS (Apple Silicon)
- .gitignore

Design decisions (made for you)
- Target architecture: aarch64 (ARM64). Rationale: your MacBook Air M4 is ARM64 so local testing is easiest and fastest. Many single-board computers and ARM servers also use aarch64.
- Userspace approach: tiny initramfs with BusyBox-based shell (built inside Docker). This keeps things simple — no kernel build required to start experimenting.
- Test method: QEMU system emulation for aarch64. It runs well on Apple Silicon and lets you iterate quickly.

Prerequisites (on your Mac)
- Docker Desktop (to build the rootfs using an aarch64 Alpine image)
- QEMU (qemu-system-aarch64). Install with Homebrew: `brew install qemu`

Quick workflow
1) Prepare a tiny rootfs and busybox (requires Docker):
   make prepare-rootfs

2) Pack the initramfs:
   make pack-initramfs

3) Boot in QEMU (you must supply an aarch64 Linux kernel image):
   make run-qemu KERNEL=/path/to/aarch64-kernel-Image

Notes on kernel image
- I did not add a kernel image to the repo. You can use a distro prebuilt aarch64 kernel (for example from a Raspberry Pi build or an ARM server distro), or build your own kernel later.
- For initial testing, look for an aarch64 "Image" or "Image.gz" you can download. The Makefile will run QEMU with the kernel you provide.

Using Raspberry Pi later
- Raspberry Pi's boot flow is different (firmware + kernel + cmdline.txt). When you're ready to deploy to Pi, I can add a build target that assembles an SD image.

Next steps
- Replace or extend the init script to start services you want (networking, SSH, etc.).
- Add a small filesystem (persisted on disk) for installing packages.
- When you want to deploy to hardware, I can add U-Boot/EFI images and instructions for Raspberry Pi or other boards.

— GitHub Copilot Chat Assistant
