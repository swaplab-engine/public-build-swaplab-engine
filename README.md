# ⚡ SwapLab Public Build Engine

![Build Status](https://img.shields.io/github/actions/workflow/status/swaplab-engine/public-build-swaplab-engine/swaplab-workflow-cache.yml?label=Public%20Runner)
![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Usage](https://img.shields.io/badge/Usage-Unlimited%20%26%20Free-green)

This repository serves as the **transparent execution engine** for **[public.swaplab.net](https://public.swaplab.net)**.

Unlike the **Sign-In version** (where builds run in *your* GitHub account), this repository hosts the public runners that execute anonymous builds for the community.

### Why use the Public Engine?

* **🛡️ No Sign-In Required:** We don't ask for your GitHub account or email.
* **📦 Unlimited Plugins:** Supports **ANY** Capacitor/Cordova Plugin or NPM package (including custom git URLs). We do not enforce a whitelist restrictions.
* **🔒 Zero-Retention:** Upload your Zip, get your APK, and we delete your files instantly.
* **🤝 Transparent & Unlimited:** Builds are executed on our **Public Engine Runner**. It is fully audit-able and **does NOT consume** your personal GitHub Action minutes.

## 📺 Video Tutorials

Visual learner? Watch our step-by-step guides directly on YouTube.

* **🎥 [Angular to Android APK: Free Public Capacitor Build + Emulator (No Sign-In)](https://youtu.be/X-gPz8twMAI)**
* **🎥 [Build Construct 3 AdMob Project: Public Capacitor Engine (No Sign-In)](https://youtu.be/QcQM4mRZ4rU)**
* **🎥 [Qwik to Android APK: Free Public Build (No Sign-In Required)](https://youtu.be/2F_4uSPvCRU)**

---

## 🏗️ Architecture: The Digital Bridge

In this workflow, **SwapLab** acts as a **Digital Bridge (Trigger)**.

It connects the [Public Dashboard](https://public.swaplab.net) to this open GitHub Workspace, allowing you to trigger builds that run directly inside this repository (`swaplab-engine/public-build-swaplab-engine`).

* **⚡️ Standard GitHub Actions:** We use standard `.yml` workflows to orchestrate the build.
* **∞ Unlimited Minutes:** Because the build runs in *our* repository, it utilizes SwapLab's organization quota.
* **🔍 Fully Auditable:** You can view the [Actions Tab](https://github.com/swaplab-engine/public-build-swaplab-engine/actions) to verify that no malicious code is injected during the build process.

---

## 🔐 Security Architecture: Battle-Tested Design

We understand that "Remote Code Execution as a Service" requires rigorous security. We employ a **"Paranoid Architecture"** to ensure safety for both the platform and the user, as validated in technical discussions with the community.

### 1. Total Isolation (Air-gapped Logic)
The Public Runners (GitHub Actions) are **physically separated** from our Private/Production infrastructure. Even if the public instance melts down or gets compromised, it isolates the failure and touches nothing else.

### 2. "Blind" Orchestrator & Clean Images
Our backend operates on a **Zero-Trust** model:
* **Clean Images:** The Docker images used are public (GHCR) and contain **zero baked-in secrets**.
* **No Local Secrets:** Our backend (`server.js`) is stateless and has no `.env` file with credentials.
* **Just-in-Time Access:** If a system key is ever needed, the container asks the Server -> the Server asks a **Cloudflare Worker** -> the Worker retrieves it from the encrypted **Secrets Store**.
* *Credentials exist in memory only for the millisecond they are needed, never stored on disk.*

### 3. Defense-in-Depth Strategy
We enforce strict multi-layered security to prevent abuse and ensure stability:
* **Front Door (Perimeter):** Our edge is guarded by **Cloudflare** enterprise tools, including **Super Bot Fight Mode**, **Turnstile** (Smart CAPTCHA), **WAF** (Web Application Firewall), and aggressive **Rate Limiting Rules**.
* **Back Door (App Level):** Application logic is hardened using industry-standard libraries like [**helmet**](https://www.npmjs.com/package/helmet) (for HTTP header security) and [**rate-limiter-flexible**](https://www.npmjs.com/package/rate-limiter-flexible) for granular traffic control.

### 4. Abuse Prevention & Resilience
We enforce strict hard-limits on CPU usage, execution time (to prevent crypto-mining), and network egress.
* **DDoS Resilient:** Our infrastructure successfully mitigated targeted attacks (e.g., Dec 11th incident) with **zero downtime**.


> **🛡️ A Note on Community Integrity:**
> SwapLab Public Engine is provided as a **100% free resource** dedicated to helping developers test and create.
>
> While our infrastructure is battle-tested, every malicious attempt to disrupt the service consumes resources meant for the community. We maintain a **Zero-Tolerance Policy** against abuse to ensure this service remains open, fast, and available for legitimate developers who rely on it.


---

## ⚙️ Configuration Guide (UI Settings)

To build your app on [public.swaplab.net](https://public.swaplab.net), you must configure **3 Mandatory Options**. These settings map directly to our public Docker images.

### 1. Project Framework Type
Select the framework that matches your source code template:
* **Capacitor**
* **Cordova**
* **Framework7**

### 2. Project File (.zip)
Upload your project folder compressed as a `.zip` file.

### 3. Image Name (Hybrid Mobile App Stack)
Select the Docker Image (Build Engine) to compile your app. These images are hosted on our [GitHub Packages Registry](https://github.com/orgs/swaplab-engine/packages).


| Image Name | Stack Description |
| :--- | :--- |
| **`capacitor-build-android-engine`** | Best for modern web apps (Ionic, React, Vue, Angular, Next.js). |
| **`framework7-build-android-engine`** | Best for Framework7 CLI projects. |
| **`cordova-build-android-engine`** | Best for legacy Cordova apps. |

> **💡 Pro Tip for Cordova & Construct 3 Users:**
> If you select **Project Framework Type: Cordova**, you maintain full flexibility. You can support **ALL** Image Name options listed above!
> * *Example:* You can build a Construct 3 game using the `capacitor-build-android-engine` to modernize the runtime.

**Note:** The **Build Type** is locked to `Debug APK` for the public service.

---

## 📂 What's Inside This Repo?

This repository contains **only** the orchestration logic required to run the build containers.

* **Workflows:** [`/.github/workflows/swaplab-workflow-cache.yml`](https://github.com/swaplab-engine/public-build-swaplab-engine/blob/main/.github/workflows/swaplab-workflow-cache.yml)
    * This is the script that pulls your temporary zip, runs the Docker container, and uploads the artifact.

**Note:** No user source code is stored in this repository. Code is injected ephemerally at runtime and destroyed immediately after.

---

## 📦 Supported Templates

This public engine is fully capable of building the entire SwapLab template ecosystem. It supports **unlimited plugins** (NPM & Cordova) without whitelist restrictions.

| Template | Description |
| :--- | :--- |
| **[Template Capacitor](https://github.com/swaplab-engine/template-capacitor)** | Modern hybrid apps using CapacitorJS runtime. |
| **[Template Cordova](https://github.com/swaplab-engine/template-cordova)** | Classic Apache Cordova apps and games. |
| **[Template Framework7](https://github.com/swaplab-engine/template-framework7)** | Native-look UI apps built with Framework7. |

---

## 🛡️ Privacy & Zero-Retention Policy

Because this is a public repository, we enforce strict privacy measures for the **Public Builder Service**:

1.  **Ephemeral Storage:** Your source code is deleted from our storage immediately after extraction.
2.  **Short-Lived Artifacts:** Build results (APKs) are automatically deleted after **1 hour**.
3.  **Masked Logs:** Sensitive inputs (like Build IDs) are masked in the public logs.
4.  **No Private Keys:** This service is for **Debug Builds only**. It does not accept private signing keystores.

For more details, please read our dedicated policies for the Public Service:

* **📜 [Terms & Conditions](https://public.swaplab.net/privacy-policy/terms-and-conditions.html)**
* **🔒 [Privacy Policy](https://public.swaplab.net/privacy-policy/privacy-policy.html)**

---

## 🚀 How to Use

You do not need to fork or clone this repository. Simply visit the dashboard to start a build:

### [👉 Go to public.swaplab.net](https://public.swaplab.net)

---
<p align="center">
  Maintained by <b>SwapLab Engineering Team</b>
</p>