
# Write Your First Web Crawler in Python Scrapy

## Introduction to Scrapy

The web scraping series would be incomplete without discussing **Scrapy**, one of the most popular frameworks for web crawling and scraping. In this post, we’ll walk you through creating a Scrapy-based web crawler to extract data from OLX's **Electronics & Appliances** section. But first, let's have a brief introduction to Scrapy.

### What is Scrapy?

From [Wikipedia](https://en.wikipedia.org/wiki/Scrapy):

> _Scrapy (/ˈskreɪpi/ skray-pee) is a free and open-source web crawling framework written in Python. Originally designed for web scraping, it can also extract data using APIs or serve as a general-purpose web crawler. It is currently maintained by Scrapinghub Ltd., a web scraping development and services company._

Scrapy provides a robust structure that simplifies the process of writing web crawlers. It handles many complex tasks, allowing you to focus on the logic of data extraction.

---

## Setting Up the Project

Scrapy introduces the concept of projects, enabling you to manage multiple crawlers (referred to as _spiders_) within a single project. This feature is particularly useful when scraping different sections of a site or multiple subdomains.

### Step 1: Create a Scrapy Project

Run the following command in your terminal to create a new Scrapy project:

```bash
scrapy startproject olx
```

This command generates a project named `olx`, with a predefined structure that includes settings, spiders, and other necessary components.

Next, navigate to your project directory:

```bash
cd olx
```

### Step 2: Create Your First Spider

To generate a spider, run the following command:

```bash
scrapy genspider electronics www.olx.com.pk
```

This will create a spider named `electronics` configured to scrape data from the `www.olx.com.pk` domain.

Your project structure should now look like this:

```
olx/
├── olx/
│   ├── spiders/
│   │   ├── electronics.py
│   ├── settings.py
│   └── ...
```

---

## Writing the Crawler

The `electronics.py` spider file contains the basic structure for your crawler:

```python
import scrapy

class ElectronicsSpider(scrapy.Spider):
    name = "electronics"
    allowed_domains = ["www.olx.com.pk"]
    start_urls = ['http://www.olx.com.pk/']

    def parse(self, response):
        pass
```

### Customizing the Spider

Let’s customize the spider to scrape data from multiple sections of the OLX Electronics & Appliances category. Update your spider as follows:

```python
from scrapy.spiders import CrawlSpider, Rule
from scrapy.linkextractors import LinkExtractor

class ElectronicsSpider(CrawlSpider):
    name = "electronics"
    allowed_domains = ["www.olx.com.pk"]
    start_urls = [
        'https://www.olx.com.pk/computers-accessories/',
        'https://www.olx.com.pk/tv-video-audio/',
        'https://www.olx.com.pk/games-entertainment/'
    ]

    rules = (
        Rule(LinkExtractor(restrict_css=('.pageNextPrev',)),
             callback="parse_item",
             follow=True),
    )

    def parse_item(self, response):
        print(f'Processing: {response.url}')
```

Key modifications:
1. **Subclassing CrawlSpider**: Makes it easier to navigate through multiple pages.
2. **Adding Rules**: Defines navigation rules for next-page links using CSS selectors.
3. **Parse Method**: A callback method to process each page’s content.

---

## Parsing Individual Items

Now, let’s extract specific details like the title, price, and URL of items. First, define an item model in `items.py`:

```python
import scrapy

class OlxItem(scrapy.Item):
    title = scrapy.Field()
    price = scrapy.Field()
    url = scrapy.Field()
```

Update the `parse_item` method to fetch individual item links:

```python
class ElectronicsSpider(CrawlSpider):
    # Existing code...

    def parse_item(self, response):
        item_links = response.css('.large > .detailsLink::attr(href)').extract()
        for link in item_links:
            yield scrapy.Request(link, callback=self.parse_detail_page)

    def parse_detail_page(self, response):
        item = OlxItem()
        item['title'] = response.css('h1::text').get().strip()
        item['price'] = response.css('.pricelabel > strong::text').get()
        item['url'] = response.url
        yield item
```

- **`response.css`**: Used to select elements using CSS selectors.
- **`yield`**: Allows the spider to handle multiple items concurrently.

---

## Storing the Data

Scrapy makes it easy to export scraped data in various formats. To save data as a CSV file, run the following command:

```bash
scrapy crawl electronics -o data.csv -t csv
```

This command stores the scraped data in a CSV file named `data.csv`.

---

## Advanced Features: Scrapy Shell

The Scrapy Shell allows you to test your scraping code interactively. For example, to test extracting data from a specific OLX item page, run:

```bash
scrapy shell https://www.olx.com.pk/item/asus-eee-pc-atom-dual-core-4cpus-beautiful-laptops-fresh-stock-IDUVo6B.html
```

Within the shell, you can execute commands like:

```python
response.css('h1::text').get().strip()
response.css('.pricelabel > strong::text').get()
```

This helps debug and refine your scraping logic without running the full crawler.

---

## ScraperAPI: Simplify Your Web Scraping

Stop wasting time on proxies and CAPTCHAs! ScraperAPI’s simple API handles millions of web scraping requests so you can focus on the data. Extract structured data effortlessly from websites like Amazon, Google, Walmart, and more.  

👉 [Start your free trial today!](https://bit.ly/Scraperapi)

---

## Conclusion

Scrapy is a powerful framework for building web crawlers and scrapers. It simplifies many challenging aspects of web scraping, enabling you to focus on extracting the data you need.

Whether you’re extracting listings from OLX or scraping other websites, Scrapy’s modular structure and rich features make it a top choice for web scraping projects.

👉 For more advanced scraping projects, consider [ScraperAPI](https://bit.ly/Scraperapi) to overcome IP bans, CAPTCHAs, and other challenges.
