**Scraping Notes - Books to Scrape**

**1. Key Decisions & Trade-Offs**

* **Two-Stage Approach:** I collected all 1,000 product URLs first before scraping individual pages. It adds a small extra step up front, but keeping link discovery separate from page parsing made debugging much cleaner.
* **Sequential vs. Parallel Requests:** I went with a single `requests.Session()` and a short 0.1-second pause between calls instead of multithreading. It took longer to run, but it kept the code simple, predictable, and easy on the server.
* **Standard CSV & `lxml`:** Using `lxml` XPath queries made pulling fields out of the product table straightforward, even if row orders shifted. Writing straight to CSV with Python's built-in `csv` module kept things lightweight.

**2. Data Volume & Runtime**
The scrape covered 1,000 books across 50 catalogue pages, totaling 1,050 HTTP requests. Reusing connections with `requests.Session()` and writing rows as they came in kept memory usage low and ensured I wouldn't lose progress mid-run. Collecting the URLs took 1m 16.8s, and scraping the book details took 6m 20.1s (about 7 minutes and 37 seconds total).

**3. What I’d Do Differently With More Time**

* **Controlled Concurrency:** I would use a `ThreadPoolExecutor` with 5–8 workers and a simple rate limiter to bring the total runtime down to around a minute without hammering the site.
* **Retries & Checkpointing:** I’d add automatic retries with exponential backoff for temporary network hiccups, plus a quick check at startup to skip URLs already saved in `books_data.csv`.

**4. Areas of Uncertainty**

* **Category Breadcrumbs:** Pulling the category from the third breadcrumb item (`<li>[3]`) assumes every page follows a strict `Home > Books > [Genre] > Title` structure. If a book didn't have a genre sub-category, this would accidentally grab the book's title instead.
* **Stock Availability Text:** My regex looks specifically for `(\d+) available`. Out-of-stock books might phrase this differently, though every single item in this run matched cleanly.

**5. Commercial Website Compliance**
Before scraping a real commercial site, I’d check `robots.txt` for disallowed paths or crawl delays, read the Terms of Service for rules on automated access, and look for an official public API first. I would stop and ask for permission if the job required logging in, bypassing paywalls or CAPTCHAs, collecting personal or copyrighted user data, or risking heavy load on their servers.