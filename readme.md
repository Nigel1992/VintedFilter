> 🚧 **COMING SOON**  
> This project will be released as a **Chrome Extension**.  
> For now, please refer to **GUIDE.md** for usage instructions.
>
> ⚠️ **Use at your own risk.**  
> This extension modifies the Vinted website locally and may conflict with Vinted’s Terms of Service.

# 🇪🇺 Vinted Country Filter – Chrome Extension

A **Chrome extension** that filters Vinted listings by **country** in real time.

It annotates items with the **country, city, and flag** next to the price, fades non-matching items, and correctly handles **lazy-loaded content while scrolling**.

---

## ✨ Features

- 🌍 **Filter by country**  
  Highlight only items from your selected country.

- 🏷️ **Live annotations**  
  Displays **country, city, and flag** next to each item’s price.

- 🔄 **Scroll-aware**  
  Automatically processes newly loaded items while scrolling.

- ⏱️ **Rate-limit safe**  
  Processes **max. 2 items per second** to avoid HTTP 429 errors.

- 🔁 **Automatic retries**  
  Retries failed API requests (429) with visible status updates.

- 🧩 **Captcha handling**  
  Automatically opens items in a new tab if a **403** response is detected, allowing the user to solve the captcha.

- 👀 **Non-intrusive filtering**  
  Non-matching items are **faded**, not removed.

- ⚙️ **Customizable**  
  Change the target country easily via a prompt.

---

## 📝 To-Do

### 1. Persist selected country
Save the chosen country so it’s automatically selected on future visits.

### 2. Country selection dropdown
Replace the prompt with a user-friendly dropdown menu.

### 3. Cache country & city data
When navigating away from Vinted and returning, previously collected data should remain visible and reusable.

---

## 🚀 Installation

1. Clone or download this repository.
2. Open Chrome and navigate to: chrome://extensions/
3. Enable **Developer mode** (top-right toggle).
4. Click **Load unpacked**.
5. Select the folder containing this extension.

---

## 📖 Usage

1. Open **Vinted** in Chrome.
2. Click the extension icon *(if implemented)*  
**or** inject the script via the browser console.
3. Enter the country you want to filter by  
(e.g. `Netherlands`).
4. Items are automatically annotated and filtered.
5. Scroll down — newly loaded items are processed automatically.

### Notes

- Items that trigger a **403 (captcha)** will open in a new tab for manual verification.
- Items hitting the **429 rate limit** are retried automatically after a short delay.

---

## 🖼️ Example — Filtering by France

<img
width="494"
height="436"
alt="Filtering by France example"
src="https://github.com/user-attachments/assets/e316242f-7c6b-401a-ad75-fe86935bd0b1"
/>

**What you see:**
- ✅ Items from the selected country remain fully visible  
- 🚫 Items from other countries are faded  
- 🏳️ Country & city info displayed with a flag  
- 🔄 Retry & captcha status shown in the annotation  

---

## 🧠 Code Overview

| File | Description |
|-----|-------------|
| `content.js` | Core logic: DOM observation, API requests, retries, filtering |
| `manifest.json` | Chrome extension configuration |
| `popup.js` *(optional)* | Future UI for country selection |
| `styles.css` *(optional)* | Styling for annotations |

---

## ⚖️ Legal Disclaimer

This project is an **independent, unofficial browser extension**.

- It is **not affiliated with, endorsed by, or sponsored by Vinted**.
- All trademarks, logos, and brand names are the property of their respective owners.
- This project is provided **for educational and personal use only**.

---

## 🔐 Data & Privacy

- This extension **does not collect, store, or transmit personal user data**.
- All processing occurs **locally in the user’s browser**.
- No analytics, tracking, or third-party services are used.
- Network requests are made **only to Vinted endpoints** already accessed by the website itself.

---

## 🚫 Terms of Service Notice

- Use of this extension is **entirely at your own risk**.
- Automated access, scraping, or modification of website behavior **may violate Vinted’s Terms of Service**.
- The author assumes **no responsibility** for account restrictions, captchas, bans, or other consequences resulting from use of this software.

---

## 🛠️ No Warranty

This software is provided **“as is”**, without warranty of any kind, express or implied, including but not limited to:

- Merchantability  
- Fitness for a particular purpose  
- Non-infringement  

In no event shall the author be liable for any claim, damages, or other liability arising from the use of this software.

---

## 🤝 Contributing

Contributions are welcome!

- Open an **issue** for bugs or feature requests
- Submit a **pull request** for improvements, UI additions, or performance optimizations

---

## 📄 License

**MIT License**

You are free to **use, modify, and distribute** this project.

