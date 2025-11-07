# 🖥️ System Monitor Tool (C++ / Linux)

A lightweight, real-time **system monitor** built in **modern C++17**, designed to display running processes, total CPU & memory usage, and per-process statistics using the Linux `/proc` filesystem.  
It features a simple **text-based UI**, supports sorting, auto-refresh, and runs seamlessly on **Linux**, **WSL**, and **Docker**.

---

## 📋 Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [How It Works](#how-it-works)
  - [/proc Filesystem](#proc-filesystem)
  - [CPU Calculation](#cpu-calculation)
  - [Memory Calculation](#memory-calculation)
- [Installation & Setup](#installation--setup)
  - [Using WSL (Ubuntu)](#using-wsl-ubuntu)
  - [Build & Run (Makefile)](#build--run-makefile)
  - [Build & Run (CMake)](#build--run-cmake)
- [Docker Support](#docker-support)
- [Usage Guide](#usage-guide)
- [Controls](#controls)
- [Screenshots](#screenshots)
- [Development Tools](#development-tools)
- [Resume Project Description](#resume-project-description)
- [STAR Interview Format](#star-interview-format)
- [Learning Resources](#learning-resources)
- [License](#license)

---

## 🚀 Overview
**System Monitor Tool** is a command-line utility similar to `top` or `htop`, built from scratch in C++.  
It reads data directly from the Linux `/proc` filesystem to display:
- Process IDs (PID)
- Process names
- CPU & Memory usage (%)
- Total system CPU and memory usage
- Sorted, real-time output

---

## ⚙️ Features
✅ Show **running processes** (PID, name, CPU %, Memory %)  
✅ Display **total CPU** and **memory** usage  
✅ **Sort** processes by CPU or memory  
✅ **Auto-refresh** every 2 seconds (configurable)  
✅ Real-time **console UI** — no GUI dependencies  
✅ **Works on Linux, WSL, and Docker**  
✅ **Clean modular structure** — easy to extend  

---

## 🧩 Tech Stack

| Component | Technology Used |
|------------|------------------|
| Language | C++17 |
| Build System | Make / CMake |
| OS Support | Linux / WSL2 |
| Containerization | Docker (Ubuntu 24.04) |
| Input Handling | POSIX (`termios`, `select`) |
| Data Source | `/proc` filesystem |

---

## 📂 Project Structure

![System Monitor Screenshot](./screenshots/monitor_view.png)

---

## 🔍 How It Works

### 📁 /proc Filesystem
The `/proc` filesystem is a **pseudo-filesystem** exposing kernel and process information.  
Each running process has its own directory `/proc/<pid>/` with files containing stats:
- `/proc/<pid>/stat` → CPU times, process name  
- `/proc/<pid>/status` → memory (`VmRSS`)  
- `/proc/stat` → total CPU usage  
- `/proc/meminfo` → total memory  

### ⚙️ CPU Calculation
1. Read `/proc/stat` to get aggregate CPU ticks.  
2. Compute delta between samples:  
   `CPU% = (Δused / Δtotal) × 100`  
3. For each process, read `utime` + `stime` from `/proc/<pid>/stat`.

### 💾 Memory Calculation
1. Read total memory from `/proc/meminfo` (`MemTotal`).  
2. Read per-process memory from `/proc/<pid>/status` (`VmRSS`).  
3. `Memory% = (VmRSS / MemTotal) × 100`.

---

## 🧰 Installation & Setup

### 🐧 Using WSL (Ubuntu)
Install required tools:
```bash
sudo apt update
sudo apt install -y build-essential g++ cmake git
