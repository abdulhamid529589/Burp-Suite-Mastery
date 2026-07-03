# Burp Suite — Complete Course Notes (Part 2)

### HTTP Protocol Fundamentals — Requests, Responses, Methods & Status Codes

_Compiled and structured from course video transcription_

---

## 📑 Table of Contents

1. [Introduction & Why HTTP Matters for Pentesting](#1-introduction--why-http-matters-for-pentesting)
2. [What Is HTTP?](#2-what-is-http)
3. [HTTP vs HTTPS — Why HTTP Is Insecure](#3-http-vs-https--why-http-is-insecure)
4. [HTTP Ports & Versions](#4-http-ports--versions)
5. [HTTP Is a Stateless Protocol](#5-http-is-a-stateless-protocol)
6. [How Sessions Are Maintained Despite Statelessness (Cookies)](#6-how-sessions-are-maintained-despite-statelessness-cookies)
7. [HTTP Request & Response — The Big Picture](#7-http-request--response--the-big-picture)
8. [Structure of an HTTP Request](#8-structure-of-an-http-request)
9. [HTTP Request Methods](#9-http-request-methods)
10. [Practical: Inspecting a Real HTTP Request (Facebook Login Page)](#10-practical-inspecting-a-real-http-request-facebook-login-page)
11. [Deep Dive: Common Request Headers](#11-deep-dive-common-request-headers)
12. [Structure of an HTTP Response](#12-structure-of-an-http-response)
13. [HTTP Response Status Codes](#13-http-response-status-codes)
14. [Deep Dive: Common Response Headers](#14-deep-dive-common-response-headers)
15. [Key Takeaways](#15-key-takeaways)

---

## 1. Introduction & Why HTTP Matters for Pentesting

This is the **second video** in the Burp Suite series. This video covers:

- What **HTTP** is.
- **How HTTP works.**
- What happens **"behind the scenes"** when a client sends a request to a server.

> **Why this matters:** If you want to pursue **bug bounty hunting** or **web application penetration testing**, understanding HTTP deeply is **absolutely essential**. Without a solid foundation in HTTP, you cannot build a strong career in web application penetration testing — this knowledge underlies almost everything you'll do with tools like Burp Suite.

---

## 2. What Is HTTP?

**HTTP** stands for **HyperText Transfer Protocol**, and is used to **transfer data across the web**.

- Most people already know:
  - **HTTP** = HyperText Transfer Protocol
  - **HTTPS** = HyperText Transfer Protocol **Secure**
  - The common (oversimplified) belief: "HTTP is insecure, HTTPS = HTTP + secure (S)."
- But there's much more to understand about HTTP than just this basic distinction.

### What HTTP Actually Does

HTTP is the **protocol** used whenever you send any request over the internet to a server — for example:

- Logging into Facebook.
- Requesting data from a Google server (e.g., searching for a cat image and receiving it).

HTTP is responsible for:

- Carrying your **request** to the server.
- Carrying the server's **response** back to you.

> In short: HTTP is the protocol that handles **transferring data across the web** — from client to server and back.

---

## 3. HTTP vs HTTPS — Why HTTP Is Insecure

- **HTTP is considered an insecure protocol** because all data transferred over HTTP is **unencrypted** — meaning data travels in **clear text (plain text)**.
- Since it's unencrypted, if someone intercepts the traffic (e.g., via a Man-in-the-Middle attack), they can **read the data directly**, because it isn't scrambled/protected in any way.

- **HTTPS**, on the other hand, uses **SSL (Secure Sockets Layer) certificates** — also called **SSL certificates**, or in modern terminology, **TLS certificates**.
  - These certificates **encrypt** the data being transferred.
  - This means the data is converted into a format that **cannot be read** by anyone intercepting it, unlike plain HTTP traffic.

> **Summary:** The core difference between HTTP and HTTPS is **encryption** — HTTPS encrypts data using SSL/TLS certificates; HTTP does not, leaving data exposed in plain text.

---

## 4. HTTP Ports & Versions

### Default Ports

| Protocol  | Default Port |
| --------- | ------------ |
| **HTTP**  | **80**       |
| **HTTPS** | **443**      |

### Version History

- The **first version** of HTTP introduced was **HTTP/0.9**.
- The **latest version** in common current use is **HTTP/1.1** (with HTTP/1.0 also still occasionally encountered).
- Today, most usage is on **HTTP/1.1**, though in some places you may still encounter references to **HTTP/1.0**.

---

## 5. HTTP Is a Stateless Protocol

**Definition (Stateless Protocol):** HTTP does **not require maintaining a connection/state** across multiple requests — **each request is independent** of the others.

### What "Stateless" Means in Practice

- HTTP does **not keep a connection permanently open**. A connection is only maintained **for the duration of a single request-response cycle**.
- Example: If your browser sends a request ("Hi") to a server, the connection stays open **only until the server's response** to that specific request arrives. Once the response is received, that particular "state" ends.
- This means: for every new request, the client needs to **tell the server again** what it wants — the server does **not inherently remember** previous interactions from the same client.

### Analogy Given in the Video

- Imagine the instructor says: _"Hello everyone, today we're going to learn about our new video."_ You (the listener) would remember this context for the rest of the conversation.
- But if this were a **stateless interaction**, after saying that sentence and getting a response ("OK"), if the instructor then continues in a "new video" context, a stateless listener would have **forgotten** what was said before and would need to be **reminded again**.
- **HTTP behaves exactly this way** — due to statelessness, information often needs to be **resent/repeated** with each new request, since the server doesn't automatically retain memory of prior requests from the same client.

> **This is why HTTP is called a "stateless protocol."**

---

## 6. How Sessions Are Maintained Despite Statelessness (Cookies)

### The Problem

If HTTP is stateless and we must repeatedly tell the server what we want with every request, **how does a session get maintained** (e.g., staying logged in, keeping items in a shopping cart)?

### The Solution: Cookies

- **Cookies** are used to **maintain your session** even though the underlying protocol (HTTP) is stateless.
- **HTTP itself is stateless, but session/state management is achieved using cookies.**
- Example: When you log into Facebook, or add an item to your cart on an e-commerce website, this "remembering" behavior depends on the **cookie** identifying which user is browsing that particular website.
- Cookies have many additional uses/attributes beyond basic session persistence — these will be covered in more detail in future videos in this series.

---

## 7. HTTP Request & Response — The Big Picture

HTTP has **two main components**:

1. **Request** — sent by the **client** to the **server**.
2. **Response** — sent by the **server** back to the **client**, in reply to the request.

### High-Level Flow

- The **client** sends a request to the server.
- The **server** processes it and sends back a response.

> Analogy used: If a client says "Hi" to a server, the server (web server) responds "Hello" back to the client — this is the fundamental request/response exchange pattern that HTTP follows.

---

## 8. Structure of an HTTP Request

### Request Components (Overview)

An HTTP request generally consists of:

1. **Command / Request Line** — includes the method, path, and protocol version.
2. **Headers** — additional metadata about the request.
3. **Body** _(optional)_ — the actual data payload being sent (present in some request types, not all).

### Detailed Breakdown of the Request Line

When you visit any webpage (e.g., logging into Facebook, or visiting a site like "MasterCyberSecurity.in"), the resulting HTTP request generally looks like this:

| Component        | Description                                                                            |
| ---------------- | -------------------------------------------------------------------------------------- |
| **Method**       | Which HTTP method is being used (e.g., `GET`, `POST`) — covered in detail in Section 9 |
| **Path**         | The specific location/resource on the server you want to access                        |
| **HTTP Version** | Which version of HTTP the browser is using                                             |
| **Headers**      | Multiple additional metadata fields (covered in Section 11)                            |

---

## 9. HTTP Request Methods

HTTP has **many request methods**. The key ones covered:

### 9.1 `GET`

- Used to **request/retrieve** any kind of resource.
- Example: A client wants to retrieve data from Facebook, or fetch/view an image from Google — the `GET` method is used.
- **`GET` requests are used only to retrieve data** — they don't submit/change server-side data.

### 9.2 `HEAD`

- Behaves like `GET`, **but without the response body**.
- It retrieves the same metadata/headers a `GET` would, but **no actual response body content** is returned.

### 9.3 `POST`

- Used to **upload/submit** data of any kind.
- **Key difference between `GET` and `POST`:** When submitting sensitive data (e.g., a login ID and password) to a website:
  - If you use the **`GET`** method, the username and password may appear **visible in the URL**.
  - If you use the **`POST`** method, the username and password are sent in the **request body**, and do **not** appear in the URL.
- **`POST` is considered significantly better/safer than `GET` for sensitive information.**

### 9.4 `PUT`

- Used to **update** any kind of data on the server.

### 9.5 `DELETE`

- Used to **delete** a specified resource.

### 9.6 `CONNECT`

- Used to **establish a tunnel** to a server defined by the target resource — i.e., used for connecting to a resource/establishing a connection.

### 9.7 `OPTIONS`

- Used when you want the target to tell you **which methods/options it supports**.
- Example: As a client, you're asking the server: "What methods do you support?" and the server might reply: "I support `HEAD`, I support `POST`," etc.

### 9.8 `TRACE`

- Used for **loopback testing** (diagnostic purposes).
- Analogy given: similar to how you might check if your own system/network address is properly working by running a `ping` command (e.g., `ping 127.0.0.1` to check loopback) and getting a reply confirming it's working correctly — `TRACE` serves a similar loopback-testing role for HTTP.

---

## 10. Practical: Inspecting a Real HTTP Request (Facebook Login Page)

### Steps to View Request Headers via Browser DevTools

1. Open a web browser and navigate to `facebook.com` (lands on the login page).
2. **Right-click** anywhere on the page → select **"Inspect Element"** (opens Developer Tools).
3. Go to the **Network** tab.
4. **Reload/re-request** the page (so the network activity is captured).
5. You'll see an entry like **"login"** in the request list — **double-click** (or click) it to open details.
6. This reveals different header categories:
   - **General Headers**
   - **Response Headers**
   - **Request Headers**

### General Headers Observed

| Field               | Meaning                                                                                     |
| ------------------- | ------------------------------------------------------------------------------------------- |
| **Request URL**     | The full URL you requested from the server (e.g., the Facebook login page URL)              |
| **Request Method**  | The HTTP method used for this specific request (e.g., `GET` observed in this case)          |
| **Status Code**     | The response status (covered fully in Section 13)                                           |
| **Remote Address**  | The remote host and port (in this case, port `443`, since `facebook.com` runs on **HTTPS**) |
| **Referrer Policy** | Explained in detail below                                                                   |

### Understanding "Referrer" and "Referrer Policy"

- A **"Referer"** header indicates **where you came from** before arriving at the current website.
- **Example:** If you're watching a video on YouTube and see an ad, and you click that ad, you land on the advertiser's website. That website can see (via the Referer header) that you came **from YouTube**.
- **Analogy given:** If you ask someone for a job referral, and they refer you to a company, when you go there, you mention "Rahul referred me here" — the company then knows you were referred by Rahul. The Referer header works the same way: it tells the destination site where the visitor was referred from.
- **Referrer policies** are configured (often for **security purposes**) to control **when/how much** referrer data is shared.
  - **Example seen on Facebook:** `strict-origin-when-cross-origin`
  - **Meaning:** Referrer data will **only be shared** if **both** the source (referring) website and the destination (target) website use the **same secure protocol level** — i.e., if both use **HTTPS**.
  - **In simple terms:** If you're on an HTTPS website and navigate to another HTTPS website, referrer data **will** be visible/sent. But if you go from an HTTPS site to a **non-HTTPS (insecure)** site, the referrer data will **not** be sent/visible.
- Other referrer policies exist as well (e.g., `no-referrer`, `origin`, etc.) — a link to detailed reading on this topic is provided in the video description for further study.

---

## 11. Deep Dive: Common Request Headers

Continuing the walkthrough of the Facebook login page's **Request Headers** section:

| Header                | Meaning                                                                                                                                                                                                                                                                                                                      |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`:authority`**      | The website/domain you went to (e.g., `www.facebook.com`)                                                                                                                                                                                                                                                                    |
| **`:method`**         | The HTTP method (e.g., `GET`)                                                                                                                                                                                                                                                                                                |
| **`:path`**           | The full path of the resource being requested (where exactly on the site you're trying to reach)                                                                                                                                                                                                                             |
| **`:scheme`**         | The protocol scheme in use (e.g., `https`)                                                                                                                                                                                                                                                                                   |
| **`accept`**          | Tells the server **what type(s) of response the client can accept/handle** (e.g., text, image, application types). If a client doesn't specify what it can accept, the server won't know what's "suitable" to send — it might send something the client can't properly render, breaking the page. **Very important header.** |
| **`accept-encoding`** | Tells the server what **compression methods** the client supports for receiving data (e.g., gzip, deflate) — data sent by the server is compressed to travel more efficiently, and this header tells the server which compression formats the client can decode                                                              |
| **`accept-language`** | Tells the server what **language(s)** the client supports/prefers                                                                                                                                                                                                                                                            |
| **`cache-control`**   | Controls **caching behavior** — explained in detail below                                                                                                                                                                                                                                                                    |
| **`user-agent`**      | Identifies details about the client's browser/OS — explained in detail below                                                                                                                                                                                                                                                 |
| **`connection`**      | Explained in detail below (`keep-alive` vs `close`)                                                                                                                                                                                                                                                                          |

### `cache-control` (Deep Dive)

- **What is a "cache"?** When you request any resource (video, image, text file) from a server (e.g., Google), a **cached copy** gets created. The next time you visit that same page/server, it loads **much faster** — this is due to caching.
- `cache-control` defines **how long** that cached data will remain valid/stored, typically measured in **seconds**.
  - Example values seen: `max-age=1600`, `max-age=80` — meaning the data will be considered valid in cache for that many seconds.
- **Why pages load faster on repeat visits:** Once a website's resources are stored in cache, subsequent requests for the **same resource** don't go directly to the server — they get served **from the cache** instead, making load times much faster.
- Related concept: **CDN (Content Delivery Network)** — the instructor offered to make a dedicated video on this topic if requested via comments.

### `user-agent` (Deep Dive)

- A header that identifies **details about the client** making the request — telling the server what platform/browser/OS the request is coming from.
- **Why it matters:** If you're logging into a website from a **mobile device** vs a **computer**, the server needs to know this so it can send appropriately formatted data. If a website designed strictly for computer screens sent that same layout to a mobile device, the page wouldn't display/work properly.
- **Purpose:** `user-agent` tells the server exactly **where the request is coming from** — e.g., "Windows 10," which OS, which browser, etc.
- **Interesting question posed to viewers:** Even though the instructor wasn't using the actual "Mozilla" browser, the `user-agent` string still commonly begins with **"Mozilla/5.0"** for historical/compatibility reasons. Viewers were invited to comment with the correct explanation for why this legacy naming persists in modern browsers' user-agent strings.

### `connection` (Deep Dive)

- In older **HTTP/1.0**, the `connection` header value was typically **`close`** — meaning the connection would close as soon as the server sent its response.
- Since **HTTP/1.1** became standard, this changed to **`keep-alive`** — allowing the connection to remain open for reuse across multiple requests (improving efficiency).
- **Rule of thumb:**
  - HTTP/1.0 → `Connection: close`
  - HTTP/1.1 → `Connection: keep-alive`

---

## 12. Structure of an HTTP Response

Just as we examined the structure of an HTTP **request**, the **response** (sent from server back to client) has its own structure:

| Component             | Description                                                                            |
| --------------------- | -------------------------------------------------------------------------------------- |
| **Version/Protocol**  | Which HTTP version is being used                                                       |
| **Status Code**       | A numeric code indicating the outcome of the request (covered in detail in Section 13) |
| **Status Message**    | A short text message corresponding to the status code                                  |
| **Headers**           | Additional response metadata                                                           |
| **Body** _(optional)_ | The actual content/data returned                                                       |

---

## 13. HTTP Response Status Codes

**Definition:** HTTP response status codes indicate **whether a specific HTTP request has been successfully completed**. Responses are grouped into **five classes**:

| Range   | Category                   | Meaning                                                                                                                                                                                                                                    |
| ------- | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **1xx** | **Informational Response** | The request was **received** and processing is **continuing**                                                                                                                                                                              |
| **2xx** | **Successful Response**    | Your action/request was **successfully received, understood, and accepted** by the server                                                                                                                                                  |
| **3xx** | **Redirection**            | The request reached the server, but the server is **further processing/redirecting** it — e.g., if you request "MasterCyberSecurity.in" but it has been redirected to a different URL, you'll see a 3xx status in that scenario            |
| **4xx** | **Client Error**           | The request contains **incorrect syntax** and cannot be fulfilled — e.g., **404** (commonly seen when a URL/webpage name is typed incorrectly), **403** (forbidden)                                                                        |
| **5xx** | **Server Error**           | The server **failed to fulfill** a request that was otherwise valid — the client sent a correct request, but there is an issue **on the server's side**. Example: a **502** error means there's some issue specifically on the server side |

> **Why this matters for pentesting:** When using Burp Suite for web application penetration testing, you will constantly see these status codes — understanding what a code starting with `1`, `2`, `3`, `4`, or `5` means is essential to correctly interpreting whether your requests are behaving as expected.

---

## 14. Deep Dive: Common Response Headers

Structure of a typical HTTP response includes elements like:

| Header/Field         | Meaning                                                                                                                                                                                              |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Method**           | Echoes which method was used                                                                                                                                                                         |
| **Status Code**      | e.g., `200` (meaning your request/response was successfully fulfilled)                                                                                                                               |
| **Date / Time**      | The date and time from the server                                                                                                                                                                    |
| **`server`**         | Tells you what **web server software** generated the response and its target — example given: `Server: Apache/2.4.14`, meaning the responding server is running **Apache web server version 2.4.14** |
| **`last-modified`**  | The date/time this resource was last modified                                                                                                                                                        |
| **`content-length`** | The length/size of the response content                                                                                                                                                              |
| **`content-type`**   | What type of content is being returned (e.g., text, image)                                                                                                                                           |
| **`connection`**     | Same `close` vs `keep-alive` concept as covered for requests (Section 11)                                                                                                                            |

### Response `cache-control` Directives

Continuing the earlier discussion of caching, several specific `cache-control` directive values were explained:

| Directive      | Meaning                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`private`**  | _(mentioned alongside other cache directives — must be valid, etc.)_                                                                                                                                                                                                                                                                                                                                                                                  |
| **`no-cache`** | If the server uses `no-cache`, it means: whenever the client requests a resource (e.g., an image), that resource must **first be re-validated with the server** before being served — **unlike normal caching**, where a previously cached copy would be served directly without contacting the server again. With `no-cache`, the request for the resource must be validated by the server first, and only then does the client receive the response |
| **`no-store`** | Means **no caching will occur at all** for that resource — every time, the response will come **directly from the server**, with nothing stored in cache                                                                                                                                                                                                                                                                                              |

### `content-security-policy` (CSP)

- **CSP (Content Security Policy)** is a header used to help **secure a website**.
- Described as a **large topic on its own** — a dedicated future video will cover it in detail. For now, just know that `content-security-policy` appears among response headers and exists for security purposes.

### `content-type`, `expires`

- `expires` behaves in an **almost identical way** to the `cache-control`/max-age concept covered earlier — essentially indicating when cached content should be considered stale.

### `strict-transport-security` (HSTS)

- Relates to securing the **transport layer**.
- **Historical context:** Previously, a tool called **"SSLstrip"** existed, which attackers used to **downgrade** an HTTPS website's connection down to plain HTTP — allowing them to read the data (since HTTP data travels unencrypted/uninterrupted/in plain format, unlike HTTPS).
- To defend against this kind of downgrade attack, the **`Strict-Transport-Security`** header is used — enforcing that browsers only connect via HTTPS, preventing downgrade to insecure HTTP.

### `x-frame-options`

- Used to **prevent clickjacking**.
- **What is clickjacking?** Example: A page might visually display "Facebook," but when you click on it, you get redirected somewhere else entirely without your knowledge/intent — this deceptive redirection is called **clickjacking**.
- To prevent this kind of attack, **`X-Frame-Options`** (and related headers) are applied.
- These topics (CSP, clickjacking, X-Frame-Options in depth) will be covered in **more detail in future videos** — only introduced briefly here.

---

## 15. Key Takeaways

- **HTTP** = HyperText Transfer Protocol, responsible for transferring data between client and server across the web.
- **HTTP is insecure** (unencrypted, plain text); **HTTPS** secures data using **SSL/TLS certificates**.
- Default ports: **HTTP = 80**, **HTTPS = 443**. Current common version: **HTTP/1.1** (originally starting from HTTP/0.9).
- **HTTP is a stateless protocol** — each request is independent; the server doesn't inherently remember previous requests. **Cookies** are the mechanism used to maintain session/state despite this statelessness.
- An HTTP exchange has two core parts: the **Request** (client → server) and the **Response** (server → client).
- **Request structure:** Method + Path + Version + Headers (+ optional Body).
- **Key HTTP methods:** `GET` (retrieve data), `HEAD` (like GET, no body), `POST` (submit/upload data — safer for sensitive info than GET), `PUT` (update data), `DELETE` (remove a resource), `CONNECT` (establish a tunnel), `OPTIONS` (ask what methods are supported), `TRACE` (loopback testing).
- **Important request headers:** `accept`, `accept-encoding`, `accept-language`, `cache-control`, `user-agent`, `connection`, `referer`/referrer-policy.
- **Response structure:** Version + Status Code + Status Message + Headers (+ optional Body).
- **Status code classes:** `1xx` = Informational, `2xx` = Success, `3xx` = Redirection, `4xx` = Client Error, `5xx` = Server Error.
- **Important response headers:** `server`, `last-modified`, `content-length`, `content-type`, `connection`, `cache-control` (with `no-cache`/`no-store` variants), `content-security-policy`, `strict-transport-security` (HSTS), `x-frame-options` (anti-clickjacking).
- This deep understanding of HTTP requests/responses/headers/status codes is **foundational** for effectively using Burp Suite in real vulnerability assessment and penetration testing work — later videos in the series will build directly on these concepts.

---

_End of Part 2 Notes — Compiled from course transcription for academic/study purposes._
