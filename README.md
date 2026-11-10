# Healthy Recipe Generator

A web page for GitHub Pages, built with HTML, CSS and JavaScript only. It needs no server, Streamlit or API key.

- Finds real recipes from [TheMealDB](https://www.themealdb.com), an online recipe database, using the ingredients you have, with a link to each original source and video
- Checks the method against your utensils, and lists anything you'd need to buy
- Asks about allergens from a list, with a box for anything else, and hides recipes that match
- Can hide pork and alcohol (halal), meat (vegetarian), animal products (vegan), desserts and deep-fried or less healthy recipes
- Estimates calories and nutrition, suggests healthier swaps and add-ons
- Downloads each recipe as an A4 PDF with photos and source links
- Falls back to 52 built-in healthy recipes if the database can't be reached

## Publish on GitHub Pages

1. Create a **public** repository on GitHub.
2. Upload every file in this folder, including `.nojekyll`, to the top level of the repository.
3. Open **Settings > Pages**.
4. Under **Build and deployment**, set **Source** to **Deploy from a branch**, the branch to **main** and the folder to **/ (root)**, then select **Save**.
5. Wait a minute or two, then open the address GitHub shows on that page.

Don't put a Gemini key in any file here. Anyone can read a GitHub Pages site's files, including keys, even when they're stored as GitHub secrets and added during publishing.

## Optional: AI alternative search

1. In [Google AI Studio](https://aistudio.google.com/apikey), create a new key.
2. Sign in to [Cloudflare](https://dash.cloudflare.com) and create a Worker from the **Hello World** template. Deploy it.
3. Select **Edit code**, replace everything with the contents of `cloudflare-worker.js`, then deploy again.
4. In the Worker's **Settings > Variables and Secrets**, add:
   - `GEMINI_API_KEY`, type **Secret**, with your key
   - `ALLOWED_ORIGIN`, type **Text**, with your site's origin, such as `https://yourname.github.io` (no path or final slash)
5. Copy the Worker's address, ending in `.workers.dev`.
6. In `config.js` on GitHub, paste the address between the quotes after `workerUrl`, then commit.

If `workerUrl` is empty, alternatives come from the built-in list, with a search link for cookbook advice.

The Worker only accepts recipe and alternative requests, from your site's origin, with a basic limit of 30 requests an hour for each visitor. Non-browser tools can fake an origin, and the limit resets when Cloudflare restarts the Worker, so this isn't complete protection. Keep billing off on the Gemini project unless you need it, so reaching the free limit stops AI rather than charging you. Visitors then get built-in recipes. Check Google's current limits and terms, as free-tier content may be used to improve Google's products.

## TheMealDB key

`config.js` uses TheMealDB's free key, `1`, which is meant for learning and personal projects. If you share the page widely, TheMealDB asks you to become a supporter and put your own key in `config.js`.

## Change the built-in recipes

Built-in recipes, foods and alternatives are in `data.js`. Extra foods for nutrition estimates are in `foods-extra.js`.

## Good to know

- **Allergens:** the check looks for known food words and some likely hidden allergens, so it can't catch everything. Always read product labels and check for cross-contamination.
- **Nutrition:** figures are estimates, not lab values or medical advice.
- **Recipes:** online recipes are shown as published by their source. Built-in recipes and alternatives were written for this page and haven't been kitchen-tested.
- **Nutrition for online recipes:** sources don't give servings or nutrition, so the page divides the whole recipe by the number of people you enter and estimates from the ingredients it recognises.
- **Halal:** the halal filter hides recipes with pork or alcohol, but you should still choose halal-certified meat and products.
- **Images:** photos come from Wikimedia Commons under free licences and are credited. They show a similar dish, and some recipes may have no photo.
- **PDF text:** PDFs use a standard font for Western European characters. Other characters in AI text appear as `?`.
