# web_scraping

This project contains the code for scraping vnexpress.net, the most popular Vietnamese website.

## Usage

After installing `scrapy` (see the Dependencies section), simply run:

```
scrapy crawl vnexpress -o data/path-to-output.json 
```
to crawl the web pages.

The extracted data will be stored in the specified file path in a json format.

## Extracted Data

You can see the data scraped in `data/vnexpress.json`. This is the result of running with the default setting and the categories specified in `crawler/vnexpress.py`.

For each article, four fields were extracted:

- `category`: the url which contains the category of the article, e.g. "https://vnexpress.net/y-kien/doi-song-p10". You should ignore the page number (`p10`) because as explaind below, the URL is not permanent and its content will change as vnexpress add more articles.
- `url`: the url to the full article, you can visit this url to read or crawl the full content
- `title`: the title of the article.
- `text`: a short exceprt of the article.

Note that some of the fields may be empty.

An example:

    "category": "https://vnexpress.net/y-kien/doi-song",
    "url": null,
    "title": null,
    "text": "Đã có nhà để ở và cho thuê, một mảnh đất, ôtô riêng, cùng hai tỷ tiết kiệm, tôi vẫn phân vân từ bỏ công việc áp lực hiện tại."

This can be read as a pandas DataFrame using:

```python
import pandas as pd
df = pd.read_json('data/vnexpress.json')
```

## Customizations

Vnexpress organizes its contents into categories. Each category may have thousands of articles, organised into numbered pages. For example, for one of the travel categories, the first page is "https://vnexpress.net/du-lich/diem-den". Subsequent pages can be formed by adding a suffix of 'p' with the page number, such as "https://vnexpress.net/du-lich/diem-den-p100". Since the articles within a page are generated dynamically, you will likely get a different result when crawling the same page url (after certain period of time depending on the frequency of the update, which seems daily at least).

### Categories

The categories can be customized by editting the `crawler/vnexpress.py` file. The categories that are currently there are for my personal purposes, you can replace with your own categories by visiting vnexpress.net.

### Pages to crawl for each category

You can choose how many pages to crawl for each category by changing the `pages` parameter in the `get_urls()` method in the same file. The default is `30` pages. Each page contains 30 articles.

## Dependencies

`scrapy` is the only dependency in this project. [The official documentation page](https://scrapy.org/) recommends installing the package using `conda` or `miniconda`.

## Scrapy settings

The settings in `spiders/settings.py` works well for me, partly because vnexpress.net did not seem to be very strict against crawling. If they have more restrictions in the future, you may need to use proxies to avoid getting blocked.