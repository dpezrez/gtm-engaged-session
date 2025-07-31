# Engaged Session tracking (GTM Custom HTML Tag)

This repository contains a **Custom HTML Tag** and supporting GTM setup for tracking **engaged sessions** based on page views and engaged time to recreate the same (or similar) concept in GA4. This tag pushes a daya layer event when the session is engaged, so you can use this as a trigger for any of your tags in GTM.

Built by [Daniel Perry-Reed](https://www.linkedin.com/in/danielperryreed/) – analytics specialist at [Data to Value](https://www.datatovalue.com).

---

## ✨ Features
- Tracks **session engagement** using:
  - **Pageview threshold** (e.g., 2+ pages)
  - **Engaged time threshold** (e.g., 10+ seconds of active time)
- Monitors:
  - **Engaged time** (ms)
  - **Total session time** (ms)
  - **Visible time** (ms)
  - **Engagement score** (0-100 score of engaged time vs visible time)
- **Pushes detailed engagement event to `dataLayer`**, including:

```json
{
  "event": "session_engaged",
  "engagementReason": "time",       // or "pageview"
  "engagedTime": 12000,
  "totalTime": 20000,
  "visibleTime": 18000
}
```

- Works with **GA4 custom events** and **custom dimensions**
- Includes **Custom JS Variables** for use in tags and triggers

---

## 📂 What’s Included
- **Option 1** manual setup: `engaged_session_tag.html` → copy and paste into a custom HTML tag
- **Option 2** simple setup: `engaged_session_container.json` → GTM conntainer export file with:
  - Custom HTML Tag for engaged session tracking
  - Custom JavaScript Variable: Engagement Score
  - Example GA4 Event Tag (`session_engaged`)
  - Example Trigger for `Custom Event: session_engaged`

---

## 🛠️ How It Works
1. The script runs on **all pages** (or your 'page view' data layer event for SPAs) and:
   - Tracks pageviews and user interactions (`scroll`, `mousemove`, `click`, `touchstart`)
   - Uses `sessionStorage` to persist data across pageviews
   - Starts a timer to measure active engagement time (pauses when user is idle or tab is hidden)
2. When **pageview threshold OR time threshold** is met:
   - Pushes an event to `dataLayer` with engagement details
3. GA4 event tag fires on this custom event, sending engagement metrics to Google Analytics.

---

## 🚀 Setup Guide

### **1. Import the GTM Container**
- Go to **GTM → Admin → Import Container**
- Choose **engaged_session_container.json** from this repo
- Merge or overwrite as needed

### **2. Publish the Workspace**
- The import includes:
  - **Custom HTML Tag** for engagement tracking
  - **GA4 Event Tag**: `session_engaged`
  - **Trigger**: Fires on `session_engaged` event
  - **Variable**: Engagement Score (0–100%)

### **3. Configure GA4**
- In GA4, create custom dimensions for:
  - `engagementReason`
  - `engagedTime`
  - `totalTime`
  - `visibleTime`
- Optionally track **Engagement Score** as a custom metric

---

## ✅ Example GA4 Event Mapping

| Parameter          | Value                                   |
|--------------------|-----------------------------------------|
| `event_name`       | `session_engaged`                     |
| `engagement_reason` | `pageview` or `time`                  |
| `engaged_time`      | Time actively engaged (ms)            |
| `total_time`        | Total session time (ms)               |
| `visible_time`      | Time page was visible (ms)            |
| `engagement_score`  | % engaged vs visible time (custom JS) |

---

## 🥪 Testing
- Use GTM **Preview Mode** to confirm:
  - The **Custom HTML Tag** fires on page load
  - `session_engaged` event appears in `dataLayer` when thresholds are met
- In GA4 **DebugView**, check for the custom event and parameters

---

## ⚠️ Notes
- Engagement logic **resets on new session** (browser tab closed or storage cleared)
- Designed for **single-tab engagement tracking** per session
- Not dependent on cookies → uses `sessionStorage`

---

## 🟦 About Data to Value

Built and maintained by myself and the team at [Data to Value](https://www.datatovalue.com) — your data activation partner helping marketing and RevOps teams transform data into predictable revenue and growth.

---

## 📄 License

MIT
