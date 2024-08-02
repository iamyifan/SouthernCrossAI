# Data Scraping Tutorial: Project Gutenberg Australia

## Project Gutenberg

[Project Gutenberg](https://www.gutenberg.org) is a volunteer-driven digital library that offers over 60,000 free eBooks, including a vast collection of classic literature, historical documents, and academic works. Founded by [Michael S. Hart](https://www.gutenberg.org/attic/hart.html) in 1971, Project Gutenberg aims to encourage the creation and distribution of eBooks by providing free access to culturally significant texts that are out of copyright.

[Project Gutenberg Australia](https://gutenberg.net.au) is a sister project that shares the same ideals and has been granted to use Project Gutenberg trademark. It hosts text files that are public domain according to [Copyright law of Australia](https://en.wikipedia.org/wiki/Australian_copyright_law), with a focus on Australian writers and books about Australia.

## Data Scraping from Project Gutenberg

### 1. Installing Scrapy

[Scrapy](https://github.com/scrapy/scrapy) is a Python library that crawls websites and extracts structured data from their pages.

To install it from [conda](https://anaconda.org/anaconda/conda): 

```sh
conda install -c conda-forge scrapy
```

To install it from [PyPI](https://packaging.python.org/en/latest/tutorials/installing-packages/): 

```sh
pip install Scrapy
```

### 2. Creating a Scrapy Project

Before scraping data from Project Gutenberg Australia, you are suggested to create a new Scrapy project:

```sh
scrapy startproject gutenberg_scraper
```
It will automatically create a `gutenberg_scraper` directory with the following contents:

```sh
gutenberg_scraper/
    scrapy.cfg            # deploy configuration file
    gutenberg_scraper/    # project's Python module, you'll import your code from here
        __init__.py
        items.py          # project items definition file
        middlewares.py    # project middlewares file
        pipelines.py      # project pipelines file
        settings.py       # project settings file
        spiders/          # a directory where you'll later put your spiders
            __init__.py
```

### 3. Preparing for Your Spider

`Spider` is a class that you define and that Scrapy uses to scrape information from a website (or a group of websites). Before defining your `Spider`,

1. Direct to `gutenberg_scraper/spiders`, create a blank script `gutenberg_spider.py` to prepare for your `Scrapy` code:

   ```sh
   cd gutenberg_scraper/spiders
   touch gutenberg_spider.py
   ```

2. Back to the project's root directory (where `settings.py` is located), create a directory `downloaded_texts` to store all scraped data:

   ```sh
   cd ../..
   mkdir downloaded_texts
   ```

### 4. Defining Your Spider

Your customised class must subclass [`Spider`](https://docs.scrapy.org/en/latest/topics/spiders.html#scrapy.Spider) and define the initial requests to make, optionally how to follow links in the pages, and how to parse the downloaded page content to extract data. Inside `gutenberg_spider.py` created from the last step, copy and paste the following code:

```python
# gutenberg_spider.py

import os
import scrapy
from urllib.parse import urljoin

class GutenbergSpider(scrapy.Spider):
    # Set the name and allowed domains for the spider
    name, allowed_domains = "gutenberg", ['gutenberg.net.au']
		# Set the starting point of the spider
    start_urls = ['http://gutenberg.net.au/']  

    def parse(self, response):
      	# Process responses and following links
        for link in response.css('a::attr(href)').getall():
            url = urljoin(response.url, link)
            # Check if the link ends with `.txt` and download it
            if url.endswith('.txt'):
                yield scrapy.Request(url, callback=self.save_text_file)
            # Otherwise follow the link if it’s within the allowed domain
            elif self.allowed_domains[0] in url:
                yield scrapy.Request(url, callback=self.parse)

    def save_text_file(self, response):
      	# Save the downloaded text file to a local directory
        file_name = response.url.split('/')[-1]
        save_path = os.path.join('downloaded_texts', file_name)
        os.makedirs(os.path.dirname(save_path), exist_ok=True)
        with open(save_path, 'wb') as f:
            f.write(response.body)
        self.log(f'Saved file {file_name}', level=scrapy.log.INFO)
```

### 5. Update Settings (Optional)

The `settings.py`  file is the central configuration file for your Scrapy project. You can update settings by modifying this file directly. For example, if you want to adjust settings such as obeying `robots.txt` or adding download delays, update the `settings.py` file in your project:

```python
# settings.py

BOT_NAME = 'gutenberg_scraper'

SPIDER_MODULES = ['gutenberg_scraper.spiders']
NEWSPIDER_MODULE = 'gutenberg_scraper.spiders'

ROBOTSTXT_OBEY = True
DOWNLOAD_DELAY = 1
CONCURRENT_REQUESTS_PER_DOMAIN = 1
```

### 6. Run Your Spider

Now, your `Spider` is good to go (scrape)! Navigate back to the project's root directory created in Step 2 (where `scrapy.cfg` is located), then run your spider:

```sh
scrapy crawl gutenberg
```

In this command, the project name must be aligned with the `name` defined in `gutenberg_spider.py`, i.e., `gutenberg` in this case. This command will start data scraping from your `allowed_domains` defined in `gutenberg_spider.py`, i.e.,  [Project Gutenberg Australia](https://gutenberg.net.au), and save scraped data into the `downloaded_texts` directory created in Step 3.

By following these steps, you should be able to create a Scrapy spider that is enable to download text files from Project Gutenberg Australia. If you encounter any issues or need further customization, feel free to ask on Discord or post an issue on GitHub.

## Other Resources

- [Installing conda](https://conda.io/docs/user-guide/install/)
- [Installing Scrapy](https://docs.scrapy.org/en/latest/intro/install.html)
- [Scrapy Tutorial](https://docs.scrapy.org/en/latest/intro/tutorial.html)

## Announcement

This tutorial is intended for research and educational purposes only. It aims to demonstrate data scraping techniques using [Project Gutenberg Australia](https://gutenberg.net.au) as an example. Please ensure that your use of data from Project Gutenberg Australia complies with their terms and conditions, and always respects copyright laws and ethical guidelines when using scraped data. If you have concerns regarding any legal implications, please visit [Copyright Act 1968](https://www.legislation.gov.au/C1968A00063/2019-01-01/text) and [Privacy Act 1988](https://www.legislation.gov.au/C2004A03712/2019-08-13/text) for more information.

This project includes content related to Australian books and cultures, including those of [Indigenous Australians](https://www.aihw.gov.au/reports-data/population-groups/indigenous-australians/overview). We acknowledge the rich cultural heritage and contributions of [Aboriginal and Torres Strait Islander peoples](https://aiatsis.gov.au/explore/indigenous-australians-aboriginal-and-torres-strait-islander-people). We are committed to engaging with this material respectfully and ethically, recognising the importance of Indigenous knowledge, history, and storytelling.

## License

[MIT](https://en.wikipedia.org/wiki/MIT_License)
