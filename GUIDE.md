# Running JavaScript in Chrome Developer Console

This guide explains how to run JavaScript code directly in the Chrome Developer Console.

## Step 1: Open Chrome Developer Tools
1. Open Google Chrome.
2. Navigate to the webpage where you want to run the code.
3. Open **Developer Tools**:
   - **Windows/Linux:** `Ctrl + Shift + I`
   - **Mac:** `Cmd + Option + I`
4. Click on the **Console** tab at the top of the Developer Tools panel.
5. Type `allow pasting` and hit Enter.

## Step 2: Run JavaScript Code [Make sure page is fully loaded]
1. In the **Console**, type or paste the following code:

```javascript

/**
 * Vinted universal filter by country (homepage + search results)
 * Processes 2 items/sec, retries 429, opens captcha tab on 403
 */

const countryToFlag = {
    "netherlands": "🇳🇱",
    "france": "🇫🇷",
    "germany": "🇩🇪",
    "belgium": "🇧🇪",
    "spain": "🇪🇸",
    "italy": "🇮🇹",
    "uk": "🇬🇧",
    "united kingdom": "🇬🇧",
    "poland": "🇵🇱",
    "sweden": "🇸🇪",
    "denmark": "🇩🇰",
    "norway": "🇳🇴",
    "finland": "🇫🇮",
    "austria": "🇦🇹",
    "switzerland": "🇨🇭",
    "ireland": "🇮🇪",
    "portugal": "🇵🇹",
    "greece": "🇬🇷",
    "hungary": "🇭🇺",
    "czech republic": "🇨🇿",
    "slovakia": "🇸🇰",
    "slovenia": "🇸🇮",
    "croatia": "🇭🇷",
    "serbia": "🇷🇸",
    "romania": "🇷🇴",
    "bulgaria": "🇧🇬",
    "estonia": "🇪🇪",
    "latvia": "🇱🇻",
    "lithuania": "🇱🇹",
    "iceland": "🇮🇸",
    "luxembourg": "🇱🇺",
    "malta": "🇲🇹",
    "cyprus": "🇨🇾",
    "ukraine": "🇺🇦",
    "russia": "🇷🇺",
    "turkey": "🇹🇷",
    "usa": "🇺🇸",
    "united states": "🇺🇸",
    "canada": "🇨🇦",
    "mexico": "🇲🇽",
    "brazil": "🇧🇷",
    "argentina": "🇦🇷",
    "china": "🇨🇳",
    "japan": "🇯🇵",
    "south korea": "🇰🇷",
    "india": "🇮🇳",
    "australia": "🇦🇺",
    "new zealand": "🇳🇿",
    "south africa": "🇿🇦"
};

const DESIRED_COUNTRY = prompt("Enter country to filter by (e.g., Netherlands):")?.toLowerCase();
if (!DESIRED_COUNTRY) {
    console.log("No country entered. Exiting.");
} else {
    console.log(`Filtering items for country: ${DESIRED_COUNTRY}`);

    const processedItems = new Map(); // itemId => {itemDiv, priceContainer, annotation, success, captchaOpened, href}
    const queue = []; // items waiting to be processed

    const processItem = (linkEl, type = 'search') => {
        const itemDiv = type === 'search' 
            ? linkEl.closest('div[data-testid="grid-item"]')
            : linkEl.closest('div.homepage-blocks__item');

        if (!itemDiv) return;

        const href = linkEl.href;

        let itemId;
        if (type === 'search') {
            const testid = linkEl.dataset.testid;
            const match = testid?.match(/product-item-id-(\d+)/);
            if (!match) return;
            itemId = match[1];
        } else {
            const match = href.match(/\/items\/(\d+)-/);
            if (!match) return;
            itemId = match[1];
        }

        if (processedItems.has(itemId)) return;

        const priceContainer = itemDiv.querySelector('[data-testid$="price-text"]');
        if (!priceContainer) return;

        let annotation = priceContainer.querySelector('.country-city-annotation');
        if (!annotation) {
            annotation = document.createElement('span');
            annotation.className = 'country-city-annotation';
            annotation.style.fontSize = '12px';
            annotation.style.fontWeight = 'bold';
            annotation.style.marginLeft = '5px';
            annotation.textContent = '⏳ Fetching country…';
            priceContainer.appendChild(annotation);
        }

        processedItems.set(itemId, {
            itemDiv,
            priceContainer,
            annotation,
            success: false,
            captchaOpened: false,
            href,
            itemId
        });
        queue.push(processedItems.get(itemId));
    };

    const retryFetch = async (item) => {
        const apiUrl = `https://www.vinted.nl/api/v2/items/${item.itemId}/details`;

        try {
            const res = await fetch(apiUrl);

            if (res.status === 429) {
                item.annotation.textContent = '⏳ Rate limit, retrying…';
                queue.push(item);
                return;
            }

            if (res.status === 403) {
                if (!item.captchaOpened) {
                    window.open(item.href, '_blank');
                    item.annotation.textContent = '⚠ Solve captcha in new tab';
                    item.captchaOpened = true;
                }
                return;
            }

            if (!res.ok) throw new Error(`HTTP error ${res.status}`);

            const data = await res.json();
            const country = data?.item?.user?.country_title_local || "";
            const city = data?.item?.user?.city || "";
            const flag = countryToFlag[country.toLowerCase()] || "";

            if (country) {
                item.annotation.textContent = ` ${flag} ${country}${city ? ", " + city : ""}`;
                item.annotation.style.color = '#2c3e50';
            } else {
                item.annotation.textContent = '❓ Unknown';
                item.annotation.style.color = 'red';
            }

            // Fade items based on filter
            item.itemDiv.style.opacity = (country.toLowerCase() === DESIRED_COUNTRY) ? '1' : '0.4';
            item.success = true;

        } catch (err) {
            console.error(`Error fetching API for item ${item.itemId}:`, err);
            item.annotation.textContent = '⏳ Fetching country…';
            queue.push(item); // retry
        }
    };

    // Queue processor: 2 items per second
    setInterval(() => {
        for (let i = 0; i < 2; i++) {
            if (queue.length === 0) return;
            const item = queue.shift();
            if (!item.success) retryFetch(item);
        }
    }, 1000);

    const processAllItems = (root = document) => {
        // Homepage items
        const homepageLinks = root.querySelectorAll('div.homepage-blocks__item a.new-item-box__overlay.new-item-box__overlay--clickable');
        homepageLinks.forEach(link => processItem(link, 'home'));

        // Search page items
        const searchLinks = root.querySelectorAll('div[data-testid="grid-item"] a.new-item-box__overlay.new-item-box__overlay--clickable');
        searchLinks.forEach(link => processItem(link, 'search'));
    };

    processAllItems();

    // MutationObserver for lazy-loaded items
    const observer = new MutationObserver((mutations) => {
        mutations.forEach(mutation => {
            mutation.addedNodes.forEach(node => {
                if (!(node instanceof HTMLElement)) return;
                processAllItems(node);
            });
        });
    });

    observer.observe(document.body, { childList: true, subtree: true });

    console.log('Vinted universal filter active: homepage + search page, 2 items/sec, 429 retry, 403 captcha.');
}
```

2. Press Enter to execute the code.

## Step 3: Check the Output

Any results, messages, or errors from your code will appear directly in the console.

You can continue interacting with the webpage using the console after running your code.
