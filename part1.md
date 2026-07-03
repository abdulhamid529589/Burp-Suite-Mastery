# Burp Suite — Complete Course Notes (Part 1)

### Introduction, Installation & Basic Configuration

_Compiled and structured from course video transcription_

---

## 📑 Table of Contents

1. [Introduction & Why Learn Burp Suite](#1-introduction--why-learn-burp-suite)
2. [Today's Agenda](#2-todays-agenda)
3. [What Is Burp Suite?](#3-what-is-burp-suite)
4. [Editions of Burp Suite](#4-editions-of-burp-suite)
5. [Alternatives to Burp Suite](#5-alternatives-to-burp-suite)
6. [How Burp Suite Works (Proxy / Man-in-the-Middle Concept)](#6-how-burp-suite-works-proxy--man-in-the-middle-concept)
7. [Installation Prerequisites](#7-installation-prerequisites)
8. [Practical: Checking/Installing Java on Windows](#8-practical-checkinginstalling-java-on-windows)
9. [Practical: Downloading & Installing Burp Suite on Windows](#9-practical-downloading--installing-burp-suite-on-windows)
10. [Burp Suite on Kali Linux (Pre-Installed)](#10-burp-suite-on-kali-linux-pre-installed)
11. [Launching Burp Suite & Project Setup](#11-launching-burp-suite--project-setup)
12. [Burp Suite Interface Overview](#12-burp-suite-interface-overview)
13. [Practical: Configuring Burp Suite Proxy](#13-practical-configuring-burp-suite-proxy)
14. [Practical: Configuring the Browser (Firefox) to Use the Proxy](#14-practical-configuring-the-browser-firefox-to-use-the-proxy)
15. [Practical: Installing Burp Suite's CA Certificate (for HTTPS Interception)](#15-practical-installing-burp-suites-ca-certificate-for-https-interception)
16. [Practical: Using the Intercept Feature](#16-practical-using-the-intercept-feature)
17. [Key Takeaways](#17-key-takeaways)

---

## 1. Introduction & Why Learn Burp Suite

This is the **first video in a Burp Suite series**. It's aimed at:

- People building a **career in cyber security**.
- People preparing for **web application penetration testing** roles.
- Anyone who wants to understand **what Burp Suite is, how it works, and how to use it** for real-world tasks (not just as a black box).

If you're preparing for **bug bounty** or **pentesting interviews/exams**, learning Burp Suite properly will be extremely helpful, since it's one of the most widely used tools by professionals in this field.

---

## 2. Today's Agenda

This video (Part 1) covers:

1. **Introduction to Burp Suite**
2. **Burp Suite Editions**
3. **Burp Suite Installation** (across different browsers and operating systems)
4. **Burp Suite Basic Configuration & Usage**

---

## 3. What Is Burp Suite?

**Definition:** Burp Suite is a **web application penetration testing tool**. In simple terms, it is used to:

- **Scan** a web application.
- **Find vulnerabilities** in it.
- **Perform reconnaissance/data gathering** on it.
- **Fix/report security issues** found in it.

### Evolution

- Originally, Burp Suite was mainly used for **scanning web applications** — finding vulnerabilities or security issues in websites.
- Today, it has become far more advanced — it's now also used to scan **mobile applications** and **APIs**, in addition to web applications.

> **In short:** Burp Suite is a web application penetration testing tool used to scan websites, mobile applications, and APIs — to find vulnerabilities, exploit them, and perform this type of testing/analysis.

- Widely used by **corporations and professionals**.
- If you're pursuing a career in web application testing, **learning Burp Suite is essential** — it will also help significantly in **interviews and certification exams**.

---

## 4. Editions of Burp Suite

Burp Suite currently comes in **three editions**, available via its official website: **portswigger.net** (link provided in the video description).

On the website, under **Products**, you'll find three options:

| Edition                  | Best For                                                                                                            | Cost                    |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------- | ----------------------- |
| **Community Edition**    | Beginners; anyone new to cyber security / web pentesting who wants to practice and test vulnerable web applications | **Free**                |
| **Professional Edition** | Professionals doing freelancing/professional work                                                                   | **Paid** (subscription) |
| **Enterprise Edition**   | Company-level/organizational use                                                                                    | **Paid**                |

- **Community Edition** is fully **free of cost** and is the recommended starting point for beginners.
- **Professional** and **Enterprise** editions require a paid subscription but offer additional features (e.g., saving/reopening projects — covered later).

---

## 5. Alternatives to Burp Suite

The market also has several **alternatives to Burp Suite**, including:

- **Zap (OWASP ZAP)**
- **Nikto**
- **Wapiti**
- **Nmap** (network-focused, related tooling)
- **Acunetix**
- A few other tools

> Note: **OWASP ZAP** is free of cost and can also be used for web app vulnerability scanning. These alternatives will be covered in more detail in separate future videos — this course focuses specifically on **Burp Suite**.

---

## 6. How Burp Suite Works (Proxy / Man-in-the-Middle Concept)

### Normal Browsing Flow (Without Burp Suite)

When a user wants to visit a website:

1. User opens a browser (e.g., Chrome).
2. Types the URL (e.g., `facebook.com`).
3. Request goes **directly** to the web server.
4. Response comes back **directly** to the user's system.

### Flow WITH Burp Suite (as a Proxy / Man-in-the-Middle)

1. User opens the browser and types a URL.
2. The request does **not** go directly to the server — it first goes to **Burp Suite**.
3. Burp Suite then **forwards** the request onward to the server.
4. All information intended for the browser passes **through Burp Suite** first.

### Key Capability

Burp Suite has the power to:

- **Hold/intercept** a request.
- **Forward** the request onward.
- **Monitor** every request going out and every response coming back.
- **Modify/manipulate** the request or response before it continues.

> **Core concept:** Burp Suite acts as a **"middleman" (Man-in-the-Middle proxy)** — every request from your system to the server, and every response back, passes through Burp Suite. This lets you monitor, analyze, and modify web traffic — which is the foundation for finding vulnerabilities, understanding website behavior, and performing security testing.

---

## 7. Installation Prerequisites

Burp Suite can be installed on:

- **Windows**
- **Linux**
- **macOS**

**Prerequisite (all OS):** The operating system must have **Java Runtime Environment (JRE)** installed, since Burp Suite requires Java to run.

---

## 8. Practical: Checking/Installing Java on Windows

### Step 1: Check if Java Is Already Installed

1. Open **Command Prompt** (`Windows key + R`, then type `cmd`, or search "cmd").
2. Run:
   ```
   java -version
   ```
3. **If you see an error** like _"java is not recognized as an internal or external command"_ → Java is **not installed**.
4. **If you see a version number** displayed → Java **is installed** and you can skip to Section 9.

### Step 2: Install Java (if not installed)

1. Go to the official Java download page (link provided in the video description).
2. Scroll down to find OS-specific download options.
3. Click on **Windows**.
4. Choose the installer matching your system architecture:
   - **64-bit** system → use the standard Windows installer.
   - **32-bit** system → look for the **x86** option (x86 = 32-bit).
5. Download will begin — once complete, **double-click** the installer file.
6. Proceed through the installation like any normal Windows software install (click **Next** → **Install** → **Close** once complete).
7. **Restart your system** after installation to ensure everything is applied correctly.
8. **Verify installation:** Open Command Prompt again and run `java -version` — it should now display the installed Java version, confirming success.

---

## 9. Practical: Downloading & Installing Burp Suite on Windows

1. Go to the **PortSwigger** website (Burp Suite's official site).
2. Navigate to **Community Edition** (since Professional requires payment).
3. Select your **platform** (Windows is typically selected by default; Linux and macOS options are also available).
4. You'll also see a **JAR file option** — this lets you run Burp Suite **directly without a full installation**, using a simple command (covered conceptually; exact command available via documentation).
5. Click **Download** for the Windows installer — the download will start automatically (if not, click the link again to trigger it manually).
6. Once downloaded, install it just like any normal Windows software (double-click → Next → Install → Finish).

> **Note:** If you use **Kali Linux**, you don't need to manually install Burp Suite — it comes **pre-installed**.

---

## 10. Burp Suite on Kali Linux (Pre-Installed)

On Kali Linux, Burp Suite is **already installed** by default — no installation steps are needed.

### Opening Burp Suite on Kali

**Option A — Via GUI:** Simply find and click the Burp Suite icon/application from the applications menu.

**Option B — Via Terminal:**

```bash
burpsuite
```

Type this command and press Enter — Burp Suite will launch. On Kali too, by default you'll see the **Community Edition** (since the Professional version requires a paid license).

---

## 11. Launching Burp Suite & Project Setup

When Burp Suite opens for the first time, you'll typically go through:

1. **Startup screen** with options like:
   - _"Delete all temporary files on exit"_ — an optional checkbox.
   - Update-related prompts (can be skipped/ignored for now).
2. **Project selection:**
   - **Professional/Paid users** can **save their projects** and reopen them later.
   - **Community Edition users** get a **"Temporary Project"** option (project data is not persisted after closing) — this is what free-tier users will work with.
   - Select **"Temporary Project"** → click **Next**.
3. Click **"Start Burp"** to launch the main application.

---

## 12. Burp Suite Interface Overview

Once launched, Burp Suite's interface shows multiple tabs/options, including (among others):

- **Target**
- **Proxy**
- **Intruder**
- **Repeater**
- _(and several more, not all covered in this introductory video)_

> Burp Suite is described as a **very large framework** with many tools within it. Mastering it thoroughly can make you a strong candidate for **bug bounty hunting** and **penetration testing** roles — it's one of the most heavily used tools in these fields, useful not just for **finding vulnerabilities** but also for general web security testing and experimentation.

> This introductory video covers only the **basics** — more advanced features will be covered in future videos in this series.

---

## 13. Practical: Configuring Burp Suite Proxy

1. Go to the **Proxy** tab.
2. Under Proxy settings, go to **Options**.
3. Here you configure the **Proxy Listener**:
   - **IP Address:** typically your **local host / loopback address** (`127.0.0.1`) — so that all requests from your browser route through this local proxy.
   - **Port:** Burp Suite's **default port is `8080`**. This is usually already set/listed by default.
   - You _can_ change the port if `8080` is already in use by something else, but by default `8080` will already be configured and active.
4. Once confirmed, this proxy (`127.0.0.1:8080`) is what you'll point your browser to.

---

## 14. Practical: Configuring the Browser (Firefox) to Use the Proxy

Since all web traffic needs to flow through the browser → Burp Suite → server, the browser itself must be configured to route its traffic through Burp Suite's proxy. Configuration steps are broadly similar across browsers; this walkthrough uses **Firefox**.

### Steps (Firefox)

1. Open **Firefox**.
2. Click the **hamburger menu** (☰) → go to **Settings/Preferences**.
3. In the settings search bar, search for **"Proxy"**.
4. This brings you to **Network Settings** → click **Settings**.
5. Select **"Manual proxy configuration"**.
6. Under **HTTP Proxy**, enter:
   - **IP:** `127.0.0.1`
   - **Port:** `8080`
     (These must exactly match Burp Suite's configured proxy listener address/port from Section 13.)
7. **Important:** HTTP-only configuration is not enough, since most websites today run on **HTTPS**. So you must **also configure the HTTPS proxy settings** the same way:
   - Use the **same IP** (`127.0.0.1`) and **same port** (`8080`) for HTTPS as well.
8. Click **OK** to save the proxy configuration.

> ⚠️ At this point, HTTP traffic will already route through Burp Suite. However, to properly intercept and decrypt **HTTPS (encrypted)** traffic, you must additionally **install Burp Suite's CA certificate** in the browser (see next section) — otherwise HTTPS sites won't load properly / their encrypted traffic can't be intercepted.

---

## 15. Practical: Installing Burp Suite's CA Certificate (for HTTPS Interception)

### Why This Is Needed

Any HTTPS website normally has a valid **SSL/TLS certificate** issued by a trusted Certificate Authority. For Burp Suite to intercept **encrypted HTTPS traffic**, your browser needs to **trust Burp Suite's own certificate** — this allows Burp Suite to sit in the middle and decrypt/inspect that traffic. Without installing this certificate, HTTPS interception will not work properly.

### Steps to Download the Certificate

1. With the proxy already configured (Section 14), open a new browser tab.
2. Navigate to:
   ```
   http://burp
   ```
3. This opens Burp Suite's built-in certificate download page (an index page hosted locally by Burp Suite itself).
4. Look for the **"CA Certificate"** option and click it.
5. Save/download the certificate file when prompted — you'll see a notification confirming the download completed.

### Steps to Install the Certificate in Firefox

1. Go back to Firefox **Settings**.
2. Search for **"Certificates"**.
3. Click **"View Certificates"**.
4. In the certificate manager, click **"Import"**.
5. Browse to and select the certificate file you just downloaded from Burp Suite, then click **Open**.
6. You'll be prompted with trust options — check the relevant boxes:
   - _"Trust this CA to identify websites"_
   - _(and the related "identify email users" option, if shown)_
7. Click **OK** to confirm.
8. Click **OK** again to finish importing — the certificate is now installed.

### Result

Once the certificate is installed, whether you visit an **HTTP** or an **HTTPS** website, **all traffic will now flow through Burp Suite** for interception and analysis.

---

## 16. Practical: Using the Intercept Feature

At this point:

- ✅ Proxy is configured (Burp Suite side + browser side)
- ✅ CA certificate is installed

Now let's see how to actually **use** Burp Suite.

### The Intercept Tab

Within the **Proxy** section of Burp Suite, there's an **"Intercept"** option/tab.

- **If Intercept is turned ON:** Any request your browser sends toward a web server will **not go directly to the server**. Instead, it will first be **held/paused inside Burp Suite**.
- The request only continues on to the server once you click **"Forward"** in Burp Suite.

### Demonstration Walkthrough

1. With Intercept **ON**, open a browser tab and type `youtube.com`.
2. The page **does not load** — because the request hasn't reached the server yet; it's being held by Burp Suite.
3. Switch to Burp Suite → you'll see the intercepted **YouTube request** waiting there.
4. Click **"Forward"** once → switching back to the browser shows the page has **partially loaded** (since only that one request/resource was released).
5. Go back to Burp Suite and keep clicking **"Forward"** for each subsequent request — each click releases one more piece of the page.
6. Once **all** requests have been forwarded, switch back to the browser — the **full YouTube page** is now completely loaded.

### What This Demonstrates

This is a simple but powerful mechanism: **any request leaving your browser is held inside Burp Suite first**, and **you decide** what happens to it next — whether to:

- Simply **inspect/check** it,
- **Send it for further vulnerability testing** to other Burp tools,
- **Modify/manipulate** the request before it's sent onward,
- Or many other possible actions (to be explored in future, more advanced videos).

### Turning Intercept Off

To disable interception (let requests pass through automatically without pausing), simply click the **Intercept** toggle button again to turn it **off**.

### Other Useful Features (Briefly Mentioned)

- **HTTP History:** Shows **every request** you've sent so far during your session from your system — a running log of all captured traffic. This is one of many other useful features Burp Suite offers beyond basic interception.

---

## 17. Key Takeaways

- **Burp Suite** = a web application penetration testing tool used to scan websites, mobile apps, and APIs for vulnerabilities.
- It works as a **proxy / man-in-the-middle** — all browser traffic routes through it, allowing monitoring, interception, and modification.
- **Three editions** exist: **Community (free)**, **Professional (paid)**, **Enterprise (paid, org-level)** — beginners should start with **Community Edition**.
- **Java (JRE)** must be installed before installing Burp Suite (except on **Kali Linux**, where Burp Suite comes **pre-installed**).
- Basic setup requires: (1) configuring **Burp's proxy listener** (`127.0.0.1:8080` by default), (2) configuring the **browser's proxy settings** to match (both HTTP and HTTPS), and (3) installing **Burp's CA certificate** in the browser to enable HTTPS interception.
- The **Intercept** feature is the core hands-on tool covered in this video — it lets you pause, inspect, and manually forward each request before it reaches the server.
- This was an **introductory/basic** video — more advanced Burp Suite usage (vulnerability scanning, Repeater, Intruder, etc.) will be covered in future parts of this series.

---

_End of Part 1 Notes — Compiled from course transcription for academic/study purposes._
