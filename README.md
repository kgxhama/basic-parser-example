
![199e2dd49ac153f1a1620afaee261e0c](https://github.com/user-attachments/assets/2f4183ee-b0a3-4877-bcf4-83e7a1984691)


# Simple Product Catalog Parser in Python

## Description

This project demonstrates a very basic web parser written in Python.  
It is intentionally simple and designed strictly for educational purposes.

The script shows how to:

- Send HTTP requests to a website
- Parse HTML content
- Extract structured data from web pages
- Handle missing values
- Iterate through multiple pages

This parser is not intended for production use.  
It does not include advanced features such as retries, proxies, rate limiting, or data storage.  
It is provided only as an example for beginners who want to understand the basic logic of web scraping.

---

# Purpose

The script loads several catalog pages from a website and extracts product information.

It collects:

- Sequential item number
- Product title
- Article number
- Brand
- Producer
- Size
- Old price
- New price

The data is printed to the console.

---

# Technologies Used

- Python 3
- requests
- BeautifulSoup (bs4)
- lxml

---

# Installation

Install dependencies before running the script:

```bash
pip install requests beautifulsoup4 lxml
````

---

# Full Script

```python
import requests
from bs4 import BeautifulSoup

# List of catalog pages
links = [
    "https://opt-opt-opt.ru/?page_size=300",
    "https://opt-opt-opt.ru/?page_size=280&PAGEN_1=2",
    "https://opt-opt-opt.ru/?page_size=280&PAGEN_1=3",
    "https://opt-opt-opt.ru/?page_size=280&PAGEN_1=4",
    "https://opt-opt-opt.ru/?page_size=280&PAGEN_1=5",
    "https://opt-opt-opt.ru/?page_size=280&PAGEN_1=6"
]

# Helper function to safely extract text
get = lambda x: x.text.strip() if x else "N/A"

# Counter for numbering items
item_number = 0

# Loop through all pages
for soup in map(lambda l: BeautifulSoup(requests.get(l).text, 'lxml'), links):

    # Find all product blocks
    for item in soup.select('.bx_catalog_item.double'):

        # Extract main elements
        a = item.select_one('.bx_catalog_item_images')
        info = item.select_one('.bx_catalog_item_articul')

        # Extract product attributes
        vals = [(s.next_sibling or '').strip()
                for s in info.select('strong')] if info else []

        # Ensure list has exactly 4 values
        vals += ["N/A"] * (4 - len(vals))

        # Increment item counter
        item_number += 1

        # Print product data
        if a:
            print(
                "Item Number:", item_number,
                "Title:", a.get('title', ''),
                "Articul:", vals[0],
                "Mark:", vals[1],
                "Producer:", vals[2],
                "Size:", vals[3],
                "Old Price:", get(item.select_one('.bx_catalog_item_old_price')),
                "New Price:", get(item.select_one('.bx_price'))
            )
```

---

# Step-by-Step Explanation

## Step 1 — Importing Libraries

The script imports two libraries:

* requests — sends HTTP requests to websites
* BeautifulSoup — parses HTML content

```
import requests
from bs4 import BeautifulSoup
```

---

## Step 2 — List of URLs

This list contains all catalog pages that the parser will process.

```
links = [
    "page1",
    "page2"
]
```

The script loops through each page one by one.

---

## Step 3 — Helper Function

The helper function safely extracts text from HTML elements.

```
get = lambda x: x.text.strip() if x else "N/A"
```

If the element exists:

```
1200
```

If the element does not exist:

```
N/A
```

This prevents errors.

---

## Step 4 — Item Counter

The counter assigns a sequential number to each product.

```
item_number = 0
```

Every time a new product is found:

```
item_number += 1
```

---

## Step 5 — Page Loading

The script loads each page using an HTTP request.

```
requests.get(url)
```

Then it parses the HTML:

```
BeautifulSoup(html, 'lxml')
```

---

## Step 6 — Finding Products

The script searches for product blocks using a CSS selector.

```
.bx_catalog_item.double
```

Example HTML:

```
<div class="bx_catalog_item double">
```

Each block represents one product.

---

## Step 7 — Extracting Elements

The script extracts:

* product title block
* product information block

```
a = item.select_one('.bx_catalog_item_images')
info = item.select_one('.bx_catalog_item_articul')
```

---

## Step 8 — Extracting Product Data

The script finds all strong tags and reads the text next to them.

Example HTML:

```
<strong>Article:</strong> 12345
<strong>Brand:</strong> BrandName
```

Result:

```
12345
BrandName
```

---

## Step 9 — Handling Missing Data

If fewer than 4 values are found, the script fills missing values.

```
vals += ["N/A"] * (4 - len(vals))
```

This prevents index errors.

---

## Step 10 — Incrementing the Counter

Each product increases the counter:

```
item_number += 1
```

---

## Step 11 — Output

The script prints the collected data.

Example output:

```
Item Number: 1
Title: Product Name
Articul: 12345
Mark: Brand
Producer: Company
Size: L
Old Price: 1500
New Price: 1200
```

---

# Limitations

This parser:

* does not use proxies
* does not use delays
* does not handle network errors
* does not store data in files
* does not support high-load scraping
* does not bypass website protections

It is intentionally simple.

# Intended Audience

This project is intended for:

* beginners learning Python
* students studying web scraping
* developers learning HTML parsing
* people who want to understand parser logic

# Possible Improvements

Future versions may include:

* saving data to CSV
* saving data to JSON
* error handling
* logging
* retry logic
* request delays
* proxy support
* automatic pagination
* multithreading

# License

This project is provided for educational purposes only.
