# Burp Suite Series (Part 3) — Setting Up a Legal Web Pentesting Lab (OWASP Juice Shop on Heroku)

> **Course:** Cyber Security — Web Application Penetration Testing
> **Topic:** Setting up a vulnerable web application target for practice (publicly hosted, legally)

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Why You Need a Dedicated Lab / Target](#2-why-you-need-a-dedicated-lab--target)
3. [What is OWASP Juice Shop?](#3-what-is-owasp-juice-shop)
4. [Overview: Local vs Public Deployment](#4-overview-local-vs-public-deployment)
5. [Step-by-Step: Deploying OWASP Juice Shop on Heroku](#5-step-by-step-deploying-owasp-juice-shop-on-heroku)
6. [Verifying the Deployment](#6-verifying-the-deployment)
7. [What You Can Do Now](#7-what-you-can-do-now)
8. [What's Coming Next in the Series](#8-whats-coming-next-in-the-series)
9. [Summary / Quick Revision Table](#9-summary--quick-revision-table)

---

## 1. Introduction

This is **Part 3** of the Burp Suite series. In this video, the focus shifts to a foundational but often-overlooked step before doing any real web application penetration testing: **setting up a proper, legal target/lab environment.**

Specifically, this video covers how to deploy a purpose-built **vulnerable web application** to a **public cloud platform**, so that it can be accessed and tested from anywhere over the internet — completely free of cost.

---

## 2. Why You Need a Dedicated Lab / Target

Before performing any kind of web application penetration testing, you need a **legal target** to test against. This is critical because:

- You **cannot** perform penetration testing / scanning / attacks on **any random website** without explicit permission.
- Doing so **without authorization is illegal** and falls under cybercrime/hacking law in most jurisdictions.
- No organization will casually grant you permission to test their live production website.

**Solution:** Set up your **own instance** of an intentionally vulnerable web application. Since you own and control this deployment, you can legally and safely practice all attack techniques (SQL injection, XSS, broken authentication, etc.) on it — without violating any laws.

Previously, such vulnerable applications were typically installed and run **locally** (e.g., inside a personal VM or local machine). This video demonstrates an alternative: hosting the same type of vulnerable application on a **public cloud platform**, making it accessible via the internet from anywhere.

---

## 3. What is OWASP Juice Shop?

**OWASP Juice Shop** is a free, open-source, intentionally **insecure web application** maintained under the OWASP (Open Web Application Security Project) umbrella. It's designed specifically for:

- Security training
- Awareness demonstrations
- CTFs (Capture The Flag challenges)
- Practicing penetration testing tools like **Burp Suite**

It intentionally contains a wide range of common web vulnerabilities (SQL injection, XSS, broken access control, etc.), making it an ideal, **safe, legal target** for hands-on practice.

---

## 4. Overview: Local vs Public Deployment

| Approach                                            | Description                                                                                                                        |
| --------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| **Local Deployment**                                | Running the vulnerable app inside your own virtual machine (VM) — accessible only on your local network                            |
| **Public Cloud Deployment (covered in this video)** | Deploying the app to a cloud platform (**Heroku**) so it gets a public URL, accessible over the internet from any device, anywhere |

**Advantage of public deployment:** You (or your team) can access and test the same target from multiple devices/locations without needing to be on the same local network or VM.

---

## 5. Step-by-Step: Deploying OWASP Juice Shop on Heroku

### Step 1: Find the OWASP Juice Shop Project

- Open your browser and search for **"OWASP Juice Shop"**.
- Click on the official project link (typically the OWASP Juice Shop GitHub/documentation page).
- This page provides full documentation — installation instructions, available features, and the different types of vulnerabilities/challenges you can attempt.

### Step 2: Explore Installation Options

- On the documentation page, look for the **installation section**.
- Multiple installation methods are listed, e.g.:
  - **From Source** — cloning the GitHub repository and running it manually
  - **Various package/container-based methods** (e.g., Docker)
  - **Deploy to Heroku** — the method used in this video

### Step 3: Select "Deploy to Heroku"

- Scroll down to find the **"Deploy on Heroku"** button/option.
- (If you're unfamiliar with what Heroku is, it's a cloud platform that lets you deploy and host web applications easily — you can search online for more general information about it.)

### Step 4: Create a Heroku Account

1. Click **Sign Up** on the Heroku deploy page.
2. Fill out the registration form:
   - Choose your **primary language**
   - Fill in the required account details
3. Click **Create Free Account**.
4. Heroku will send a **verification link** to your registered email address.
5. Open your email and click the verification link to **verify your account**.
6. Once verified, your Heroku account setup is complete.

### Step 5: Return to the Juice Shop Deployment Page

- Go back to the **OWASP Juice Shop** page (where the "Deploy to Heroku" link was found).
- Click the **"Deploy to Heroku"** option again.

### Step 6: Log In and Configure the App

1. Log in with the Heroku account you just created.
2. You'll be asked to provide an **App Name**.
   - Choose any name you like (e.g., a custom identifier for your instance).
   - ⚠️ **Note:** The app name must be **unique** — if your chosen name is already taken, you'll need to pick a different one.
3. Click **Deploy App**.

### Step 7: Wait for Deployment

- Heroku will take some time to **build and deploy** the full application onto its platform.
- Once the deployment process completes, you'll see a confirmation that the app has been successfully deployed.

### Step 8: Open Your Deployed Application

- Click the **"View"** / **"Open App"** button.
- This will launch your newly deployed OWASP Juice Shop instance in the browser, now running on a **public URL**.

---

## 6. Verifying the Deployment

- Once opened, you should see the OWASP Juice Shop web application's homepage — fully functional, just like the original, but now hosted under **your own app name** on Heroku.
- You can verify this by checking the app name you provided earlier — it should appear as part of the deployment URL/dashboard on Heroku.
- The web application is now **fully live and publicly accessible**.

---

## 7. What You Can Do Now

Because this instance of OWASP Juice Shop:

- Is **your own deployment**, and
- Is an **intentionally vulnerable application built specifically for security testing**,

...you can now **legally and freely** perform any kind of penetration testing / attack technique on it — without needing anyone's permission, since you are the owner of this deployment.

This makes it a perfect **practice target** for tools like **Burp Suite**.

---

## 8. What's Coming Next in the Series

- **Next video:** How to set up a similar **vulnerable web application (DVWA – Damn Vulnerable Web Application)** locally, on a **Windows machine** or a **Linux machine**.
- **Future videos:** How to actually **use Burp Suite** to perform various attacks against these vulnerable web applications, to properly learn practical web application penetration testing techniques.

---

## 9. Summary / Quick Revision Table

| Concept                        | One-Line Summary                                                                                                                                                        |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Why a lab is needed**        | Testing random websites without permission is illegal; you need your own authorized target                                                                              |
| **OWASP Juice Shop**           | A free, intentionally vulnerable web app built for security training and practice                                                                                       |
| **Local deployment**           | Running the vulnerable app inside your own VM, accessible only locally                                                                                                  |
| **Public deployment (Heroku)** | Hosting the app on Heroku for free, giving it a public URL accessible from anywhere                                                                                     |
| **Heroku**                     | A cloud platform used to deploy and host web applications easily                                                                                                        |
| **Deployment steps**           | Find project → check installation docs → click "Deploy to Heroku" → sign up/verify Heroku account → return & deploy → name the app → wait for build → open the live app |
| **Legal note**                 | Because you own the deployment, you can legally test/attack it without needing external permission                                                                      |
| **Next steps in series**       | Setting up DVWA locally (Windows/Linux) + using Burp Suite to perform real attacks                                                                                      |

---

_End of Notes — Burp Suite Series, Part 3_
