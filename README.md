# Online‑Exam‑Verification: Candidate Device Environment Verification Sheet
> Pure‑frontend offline device verification tool for online examinations. **All computations, feature collection and image processing run locally inside the browser. No data is uploaded to any remote server.**
> File: `index.html`. Open directly in browser. No backend or server deployment required.

## Project Overview
This single‑page web application performs local‑side device environment verification for exam candidates.
After entering candidate name and class ID, the page automatically runs a full verification workflow:
- Detect hardware, operating system, browser, screen, network and time‑zone information
- Collect multi‑dimensional browser device fingerprints including WebGL, Canvas, audio context and system fonts
- Capture mouse / touch pointer trajectory samples and perform behavioural analysis to identify potential automated‑operation patterns
- Validate camera permissions, capture live video and take watermarked candidate self‑portrait photos
- Auto‑generate verification verdict: `Passed` / `Passed with Risks` / `Failed`
- Export a complete standalone HTML verification report for local archiving, printing or examiner review

> All data remains in browser memory. The final report is saved as a local file by user action. Works under local `file://` protocol.

## Feature List
1. **Candidate Information Entry**
    - Name validation: 2‑20 characters (Chinese / Latin alphabet)
    - Class ID validation: 5‑digit number, first three digits = cohort number, last two digits = class number
2. **Basic Environment Detection**
    - Auto‑classify device type: Desktop / Laptop / Tablet / Smartphone / Wearable device
    - Detect operating system, browser rendering engine, viewport, display metrics, touch capability, network status, time zone & locale, security context
3. **Browser Device Fingerprint Collection**
    - WebGL GPU renderer and vendor information
    - Canvas fingerprint, audio‑context fingerprint
    - System font enumeration, speech‑synthesis voice count
    - Battery status, storage quota, keyboard layout, peripheral APIs and browser plugin enumeration
    - Generate composite hashed device fingerprint for device consistency cross‑check
4. **Pointer Behaviour Sampling & Analysis**
    - Capture mouse / touch movement trajectory and click events on a canvas area
    - Calculate movement speed, direction changes, travel distance and click count
    - Analyse keystroke timing and speed variance; flag potential script‑driven automated behaviour
    - Embed trajectory snapshot image into final exported report
5. **Camera Verification & Self‑portrait Capture**
    - Request camera permissions, enumerate and switch between available video input devices
    - 3‑second countdown capture; auto‑apply watermark containing candidate metadata to photos
    - Mobile fallback: invoke native system camera when real‑time camera API is blocked under local `file://` mode
    - Allow skipping camera step (marks anomaly while still generating report for manual review)
6. **Automated Verdict Generation**
    - Block smartphones and smart‑watches from passing verification
    - Flag risks such as UA‑hardware mismatch and suspected automated input
    - Three‑tier result: `Passed` / `Passed with Risks` / `Failed` with stylized red seal graphic
7. **Export Standalone Offline Report**
    - Export self‑contained HTML report including all detection logs, fingerprints, trajectory graph, self‑portrait photo and verification seal
    - Preview the report in a new window before archiving; report requires no internet connection; viewable and printable in any modern browser
    - A leave‑confirmation dialog appears if the tab is closed before the report has been downloaded, preventing accidental data loss (all data lives in volatile memory)

## File Structure
```
Online‑Exam‑Verification/
├── index.html                 # Complete source code (HTML + CSS + JavaScript single‑file, zero business‑logic external dependencies)
├── assets/images/logo.png     # Favicon
└── README.md                  # This documentation
```
> All styles and scripts are inline. Only external resources are Google Fonts for UI typography.
> In fully offline intranet environments, features keep working normally; only font rendering degrades to system default fonts.

## Quick Start
### Option 1: Open locally (Recommended)
1. Save `index.html` to your local computer or mobile device.
2. Open the file with a modern browser (Chrome / Edge preferred. Some APIs have limitations in Firefox and Safari).
3. Fill in candidate name and 5‑digit class ID, click *Register & Start Device Verification*.
4. Follow on‑screen steps to finish environment checks, behaviour sampling and self‑portrait capture.
5. After completing all steps, download and save the HTML verification report locally.

> ⚠️ Mobile `file://` limitation: Most mobile browsers block real‑time camera access under local‑file protocol. Use the *Invoke System Camera (Mobile Fallback)* button as an equivalent alternative.

### Option 2: Static Web Server Deployment
Host `index.html` on any static web server (Nginx, Apache or static hosting service). **HTTPS is mandatory**, as modern browsers block camera API under unencrypted HTTP connections.

## Candidate Workflow
1. Open the page. Enter real full name and 5‑digit class ID (Example: `10501` = Cohort 105, Class 01). Complete registration.
2. The page automatically runs *Basic Environment Detection* and *Device Fingerprint Detection*. Wait for completion.
3. **Behaviour Sampling**: Move your mouse or finger freely inside the sampling canvas area and perform several clicks or taps for approximately 10 seconds.
4. **Camera Verification**: Grant browser camera permission. For local‑file mobile usage, use the native system‑camera fallback option.
5. **Self‑portrait Capture**: Face the camera, trigger capture with 3‑second countdown. Confirm your photo.
6. View final verification verdict. Download the HTML report for archiving or examiner submission.

## Limitations & Important Notes
1. **Privacy Statement**
    > This tool does **not** upload any photos, fingerprints or candidate information over the network. All processing occurs locally within browser memory. Data persists on disk **only after you manually download the exported HTML report**.
2. **Browser Compatibility**

| Browser | Compatibility Notes |
|---|---|
| Chrome / Edge | ✅ Full feature support, recommended |
| Firefox | Partial fingerprint APIs restricted; core workflows functional; some items show "Unreadable" |
| Safari (macOS / iOS) | Hardware fingerprint APIs are system‑limited. iOS `file://` cannot access real‑time camera; use system‑camera fallback only. |

3. Smartphones result in an automatic `Failed` verdict as they are disallowed for online examinations. Tablets trigger risk warnings.
4. Google Font network dependency: In air‑gapped offline environments, functionality remains intact, only typography appearance changes.
5. This tool provides **auxiliary reference only**. It cannot fully replace human proctoring. Reports marked with risk flags require manual examiner review.

## Intranet / Air‑gapped Adaptation
- Remove all `[https://fonts.googleapis.com](https://fonts.googleapis.com)` font links for complete zero‑external‑resource offline deployment.
- No database or backend interfaces required. Pure static single‑file application.

## Report Content
The exported `.html` report includes:
- Candidate metadata, verification ID, timestamp and graphical verification seal
- Full basic environment detection table
- Device fingerprint and browser feature details
- Behaviour‑sampling metrics, automated analysis conclusion and trajectory image
- Camera status information and watermarked candidate self‑portrait
- Composite device fingerprint hash string and raw User‑Agent string

This HTML file can be printed, archived or forwarded to exam administrators.

## Disclaimer
This is a demonstration‑grade single‑file frontend tool built for online‑exam device‑validation scenarios.
End‑users assume full responsibility for all usage risks.
No end‑user data is collected. There are no server‑side components or telemetry trackers.

## FAQ
**Q: Camera shows black / grey screen on mobile?**
A: Real‑time camera API is restricted by mobile browsers under local `file://` protocol. Use *Invoke System Camera (Mobile Fallback)* button to take photos with device native camera.

**Q: Where is my test data stored?**
A: Data lives only in browser volatile memory and is destroyed when you close the tab. Data is written to local disk **only when you manually download the HTML report**.

**Q: Can I run this with zero internet access?**
A: Yes, open `index.html` locally. Google Fonts will fail to load, but all core functions work as intended.

**Q: Why does tablet return "Passed with Risks"?**
A: Tablets support split‑screen and multi‑tasking operations which create cheating opportunities. Risk flag requires human examiner review.

**Q: What does "Suspected automated behaviour" mean?**
A: Highly constant pointer speed, minimal direction variance or perfectly regular keystroke intervals match typical characteristics of script‑driven input. This is a heuristic warning and not definitive proof of misconduct; manual review is required.