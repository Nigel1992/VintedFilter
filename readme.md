# COMING SOON AS AN EXTENTION FOR CHROME, USE GUIDE.MD FOR NOW

# Vinted Country Filter Chrome Extension

A **Chrome extension** that filters Vinted items by country in real-time.  
It highlights or annotates items with the country, city, and flag next to the price, fades non-matching items, and handles lazy-loaded content while scrolling.

---

## Features

- **Filter by country**: Only highlight items from your desired country.  
- **Live annotation**: Shows country, city, and country flag next to the price.  
- **Scroll-aware**: Automatically detects newly loaded items as you scroll.  
- **Rate-limit safe**: Processes a maximum of 2 items per second to avoid 429 errors.  
- **Automatic retry**: Retries failed API requests (429) with informative status.  
- **Captcha handling**: Opens items in a new tab if a 403 response occurs, prompting the user to solve captcha.  
- **Non-intrusive**: Non-matching items are faded, not removed.  
- **Customizable**: Easily change the country to filter via a prompt.

---

## TO-DO

1. Remember selected country
Save the chosen country so it is automatically selected on the next visit.

2. Country selection dropdown
Use a dropdown menu to easily choose a country.

3. Save country and city data when navigating
When you visit other pages on Vinted and then come back, the previously collected country and city data will still be displayed and used again.

---

## Installation

1. Clone or download this repository.
2. Open Chrome and go to `chrome://extensions/`.
3. Enable **Developer mode** (toggle in the top-right corner).
4. Click **Load unpacked** and select the folder containing this extension.

---

## Usage

1. Open Vinted in Chrome.
2. Click the extension icon (if implemented) or inject the script via the console.
3. Enter the country you want to filter (e.g., `Netherlands`) in the prompt.
4. The feed will automatically annotate items with country info and fade non-matching items.
5. Scroll down to load more items — new items are processed automatically.

**Notes:**
- Items triggering a 403 (captcha) will open in a new tab for manual verification.
- Items that exceed the rate limit (429) are automatically retried every few seconds.

---

## Example by filtering France

<img width="494" height="436" alt="image" src="https://github.com/user-attachments/assets/e316242f-7c6b-401a-ad75-fe86935bd0b1" />
  

- Items from the desired country remain fully visible.  
- Items from other countries are faded.  
- Country and city information displayed with a flag.  
- Retry and captcha status displayed on the item annotation.

---

## Code Overview

- `content.js`: Main script handling DOM observation, API requests, retries, and filtering.  
- `manifest.json`: Chrome extension configuration.  
- `popup.js` (optional): For future enhancements like country selection UI.  
- `styles.css` (optional): For styling annotations.

---

## Contributing

Contributions are welcome! Feel free to submit issues or pull requests to improve functionality, add UI, or optimize performance.

---

## License

MIT License  
Feel free to use, modify, and distribute this extension.
