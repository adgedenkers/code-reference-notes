# Python Automated Real Estate Data Pipeline Project: Part 1 – Web Scraping

In this four-part project series, we’ll build an automated real estate data pipeline that scrapes property listings, cleans the data, visualizes trends, and tracks historical prices over time.

The goal is twofold. First, it explores the fundamentals of web scraping. That's a valuable tool for many industries, especially when gathering data. The second major goal is to demonstrate real-world knowledge and practical skills. 

Whether you're building a programming portfolio or just honing your web-scraping skills, you'll find a detailed walkthrough with this four-part Python project.

## Why This Project?

This Python project is truly portfolio-worthy as it allows you to showcase essential real-world automation skills using Python. By the end of this tutorial series, you'll have a tool that can:

- **Scrape live real estate data** automatically.

- **Analyze and clean** property details.

- **Visualize data trends** using an interactive dashboard.

- **Track historical price changes** for long-term analysis.

## Project Breakdown

This is a fairly massive project, so it makes sense to tackle it in four separate parts:

1 - **Web Scraping** – Extracting real estate data with Selenium.

2 - **Data Cleaning & Analysis** – Processing and analyzing property listings.

3 - **Dashboard with Streamlit** – Visualizing trends interactively.

4 - **Automation & Historical Tracking** – Running the scraper automatically and tracking long-term trends.

## Part 1: Web Scraping Real Estate Data with Selenium

In the first part of our [Python project](https://hackr.io/blog/python-projects), we’ll scrape property listings from Redfin using Selenium. We're going to extract a range of essential details, including prices, addresses, beds/baths, images, and geo-coordinates. We'll then save this information in a structured dataset for analysis (which we'll cover in part 2).

## Prerequisites

Don’t worry if you’re not a [Python](https://hackr.io/blog/what-is-python) expert, but before diving into the code, checking that you have these skills under your belt will make this journey smoother and more enjoyable.

### Basic Python Knowledge

You should be familiar with:

- Python functions and loops.
- Working with Pandas for data handling.
- Basic web scraping concepts (HTML structure, CSS selectors).

### Required Libraries

We'll be using the following Python packages:

|Library|Purpose|
|---|---|
|`selenium`|Automates web browsing to extract real estate data|
|`webdriver_manager`|Manages browser drivers for Selenium|
|`pandas`|Stores and processes scraped data|
|`json`|Parses structured listing details|
|`random` & `time`|Introduces delays to avoid detection|

Show more

### Install Dependencies

Before we fire up our editor and start coding, make sure you have all of the required libraries by running this command:

```python
pip install selenium webdriver-manager pandas
```

### Set up Your Project

Before we start coding, let’s set up the project:

1. Make sure Python is installed on your computer. If not, download it from the [official Python website](https://www.python.org/).  
2. Open your favorite code editor or IDE.  
3. Create a new Python file, for example, _scraper.py_.

Great, now, let's dive head first into our [Python editor](https://app.hackr.io/editors/python) to get this build started.

## Step 1: Setting Up Selenium & Firefox WebDriver

To handle web scraping in this Python project, we'll be using Selenium.

This is a powerful tool for automating browser interactions and it allows us to extract dynamic content from real estate listings, just as a human user would.

```python
from selenium import webdriver
from selenium.webdriver.firefox.service import Service
from selenium.webdriver.firefox.options import Options
from selenium.webdriver.firefox.firefox_profile import FirefoxProfile # Optional
from webdriver_manager.firefox import GeckoDriverManager
```

### Why Selenium?

Many modern websites use JavaScript to load data dynamically. Simple scrapers like `requests` and `BeautifulSoup` only fetch static content, missing key elements. Selenium solves this by simulating browser actions and capturing fully rendered pages.

To run Selenium, we need a **WebDriver** —a tool that acts as the bridge between Python and the web browser. Lucky for us, `webdriver_manager` automatically installs and manages the required browser drivers for us.

## Step 2: Implementing Stealth Settings

Many websites try to detect and block automated scripts. We can reduce this risk by:

- Randomizing our **User-Agent** with [Python random](https://hackr.io/blog/python-random-function) (so our script appears like different browsers).
- Running the browser in **headless mode** (without opening a visible window).
- Disabling automation flags that websites check for with anti-social bots.
- Using a user profile for our headless browser (Optional)

```python
import random
import json
import time

USER_AGENTS = [
    "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36",
    "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.0.0 Safari/537.36",
    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.0.0 Safari/537.36",
    "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Firefox/119.0",
]

# OPTIONAL: Update with your actual Firefox profile path
profile_path = "/home/your_name/.mozilla/firefox/abcdefgh.your-profile"

options = Options()
options.profile = FirefoxProfile(profile_path) # OPTIONAL
options.add_argument("--headless")  # Run in background (no GUI)
options.set_preference("dom.webdriver.enabled", False)
options.set_preference("useAutomationExtension", False)
options.set_preference("general.useragent.override", random.choice(USER_AGENTS))

service = Service(GeckoDriverManager().install())
driver = webdriver.Firefox(service=service, options=options)
```

### Ethical Web Scraping & Best Practices

One thing I have to cover is that many websites actively detect and block automated scripts. To avoid being flagged as a bot and ensure **ethical scraping**, follow these best practices:

- **Respect the website’s** `**robots.txt**` **file** – Some sites explicitly prohibit scraping.
    
- **Don’t overload the server** – Introduce random delays between requests (1-10 seconds) to mimic human behavior.
    
- **Avoid excessive requests** – Scrape only the data you need.
    
- **Rotate User-Agents and IPs** – This helps prevent detection and IP bans.
    
- **Never violate a website’s terms of service** – Always ensure your use case is legally and ethically sound.
    

I've tried to adhere to all of these practices in this project, and I'd recommend you do the same in any future web scraping endeavors!

## Step 3: Scraping Real Estate Listings

### Define Target URL

For fun, we're going to scrape properties in the Hollywood Hills from Redfin:

![Web scraping from Redfin for real-estate data](https://cdn.hackr.io/uploads/posts/attachments/1738345339nBDppkQD2p.webp)

To do that, we need to get the URL for the search results page and then use this with our WebDriver.

We'll also add a basic check to see if the page title indicates whether we've been blocked from accessing the page. This should not be an issue if we're following ethical practices, but I've added it here for completeness.

```python
base_url = "https://www.redfin.com/neighborhood/547223/CA/Los-Angeles/Hollywood-Hills"
driver.get(base_url)
time.sleep(random.uniform(5, 8))  # Random delay

# Check if we are being blocked
print(f"Page title: {driver.title}")
if "Access" in driver.title or "blocked" in driver.page_source.lower():
    print("WARNING: Redfin has blocked the request!")
    driver.quit()
    exit()
```

### Extract Property Listings

To collect real estate data, we:

1. Find the listings container and listings within this container, using a [try-except](https://hackr.io/blog/python-try-except) block to exit if we're unable to locate any listing data.
2. Extract price, beds, baths, square footage, image links, and geolocation data with a range of try-except blocks and fallback values for blank values.
3. Extract the address and links with a range of try-except blocks with continue statements to skip the iteration if these critical values are missing
4. Locate and click on the "Next" button to navigate through pages.

Notice how we're using a [Python while loop](https://hackr.io/blog/python-while-loop) that only breaks after we fail to find a next page button, as this indicates we're on the last page of results.

**Important:** The CSS selectors I have used below were working at the time of creating this project in January 2025. But, website page structures can change all the time, so be sure to double-check the page source code to ensure these classes are still applicable.

```python
scraped_data = []
page_number = 1

while True:
    print(f"Scraping page {page_number}...")

    # Locate the main listings container
    try:
        container = driver.find_element("css selector", "div.HomeCardsContainer")
        listings = container.find_elements("css selector", "div.HomeCardContainer")
    except:
        print("Failed to locate the property list container. Exiting...")
        break

    print(f"Found {len(listings)} listings on page {page_number}")

    for listing in listings:
        # Extract price
        try:
            price = listing.find_element("css selector", "span.bp-Homecard__Price--value").text.strip()
        except:
            price = "N/A"

        # Extract address
        try:
            address = listing.find_element("css selector", "div.bp-Homecard__Address").text.strip()
        except:
            print("Skipping a listing due to missing address data")
            continue  # Skip listings with missing elements

        # Extract beds, baths, and sqft
        try:
            beds = listing.find_element("css selector", "span.bp-Homecard__Stats--beds").text.strip()
        except:
            beds = "N/A"

        try:
            baths = listing.find_element("css selector", "span.bp-Homecard__Stats--baths").text.strip()
        except:
            baths = "N/A"

        try:
            sqft = listing.find_element("css selector", "span.bp-Homecard__LockedStat--value").text.strip()
        except:
            sqft = "N/A"

        # Extract listing link
        try:
            link = listing.find_element("css selector", "a.link-and-anchor").get_attribute("href")
            link = f"https://www.redfin.com{link}" if link.startswith("/") else link
        except:
            print("Skipping a listing due to missing link data")
            continue  # Skip listings with missing elements

        # Extract image URL
        try:
            image_element = listing.find_element("css selector", "img.bp-Homecard__Photo--image")
            image_url = image_element.get_attribute("src")
        except:
            image_url = "N/A"
        
        try:
            # Extract Geo-Coordinates (Latitude & Longitude)
            json_script = listing.find_element("css selector", "script[type='application/ld+json']").get_attribute("innerHTML")
            json_data = json.loads(json_script)

            latitude = json_data[0]["geo"]["latitude"]
            longitude = json_data[0]["geo"]["longitude"]
        except:
            latitude = "N/A"
            longitude = "N/A"

        # Store the data
        scraped_data.append({
            "Price": price,
            "Address": address,
            "Beds": beds,
            "Baths": baths,
            "SqFt": sqft,
            "Link": link,
            "Image URL": image_url, 
            "Latitude": latitude,
            "Longitude": longitude
        })

    # Pagination: Check if a "Next Page" button exists
    try:
        next_button = driver.find_element("css selector", "button.PageArrow__direction--next")
        next_button.click()
        page_number += 1
        time.sleep(random.uniform(5, 10))  # Random delay before next request
    except:
        print("No more pages. Scraping complete.")
        break  # Exit loop when no next page exists
```

## Step 4: Saving the Data

After our loop terminates and we have collected our raw data, we will convert it into a Pandas dataframe. We can then store this extracted data in a CSV file for further analysis in part 2 of our project. I am adding this to a _data_ directory within my main project directory to keep it organized.

It's also important that we close our WebDriver after we've finished scraping our data.

```python
import pandas as pd
df = pd.DataFrame(scraped_data)
df.to_csv("data/redfin_hollywood_hills.csv", index=False)
print(f"{len(df)} listings saved!")

driver.quit()
```

## Full Program Source Code

```python
from selenium import webdriver
from selenium.webdriver.firefox.service import Service
from selenium.webdriver.firefox.options import Options
from selenium.webdriver.firefox.firefox_profile import FirefoxProfile
from webdriver_manager.firefox import GeckoDriverManager
import pandas as pd
import time
import random
import json

# Define a list of User-Agents to rotate
USER_AGENTS = [
    "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36",
    "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.0.0 Safari/537.36",
    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.0.0 Safari/537.36",
    "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Firefox/119.0",
]

# Update with your actual Firefox profile path
profile_path = "/home/your-name/.mozilla/firefox/abcdefgh.your-profile"

# Setup Firefox options for stealth mode
options = Options()
options.profile = FirefoxProfile(profile_path)
options.add_argument("--headless")  # Run in background (no GUI)
options.add_argument("--disable-gpu")
options.set_preference("dom.webdriver.enabled", False)
options.set_preference("useAutomationExtension", False)

# Randomly select a User-Agent
user_agent = random.choice(USER_AGENTS)
options.set_preference("general.useragent.override", user_agent)

# Use WebDriver Manager to install Geckodriver
service = Service(GeckoDriverManager().install())
driver = webdriver.Firefox(service=service, options=options)

# Redfin search URL for Hollywood Hills, Los Angeles
base_url = "https://www.redfin.com/neighborhood/547223/CA/Los-Angeles/Hollywood-Hills"

# Initialize data storage
scraped_data = []

# Start scraping
driver.get(base_url)
time.sleep(random.uniform(5, 8))  # Random delay

# Check if we are being blocked
print(f"Page title: {driver.title}")
if "Access" in driver.title or "blocked" in driver.page_source.lower():
    print("WARNING: Redfin has blocked the request!")
    driver.quit()
    exit()

page_number = 1
while True:
    print(f"Scraping page {page_number}...")

    # Locate the main listings container
    try:
        container = driver.find_element("css selector", "div.HomeCardsContainer")
        listings = container.find_elements("css selector", "div.HomeCardContainer")
    except:
        print("Failed to locate the property list container. Exiting...")
        break

    print(f"Found {len(listings)} listings on page {page_number}")

    for listing in listings:
        # Extract price
        try:
            price = listing.find_element("css selector", "span.bp-Homecard__Price--value").text.strip()
        except:
            price = "N/A"

        # Extract address
        try:
            address = listing.find_element("css selector", "div.bp-Homecard__Address").text.strip()
        except:
            print("Skipping a listing due to missing address data")
            continue  # Skip listings with missing elements

        # Extract beds, baths, and sqft
        try:
            beds = listing.find_element("css selector", "span.bp-Homecard__Stats--beds").text.strip()
        except:
            beds = "N/A"

        try:
            baths = listing.find_element("css selector", "span.bp-Homecard__Stats--baths").text.strip()
        except:
            baths = "N/A"

        try:
            sqft = listing.find_element("css selector", "span.bp-Homecard__LockedStat--value").text.strip()
        except:
            sqft = "N/A"

        # Extract listing link
        try:
            link = listing.find_element("css selector", "a.link-and-anchor").get_attribute("href")
            link = f"https://www.redfin.com{link}" if link.startswith("/") else link
        except:
            print("Skipping a listing due to missing link data")
            continue  # Skip listings with missing elements

        # Extract image URL
        try:
            image_element = listing.find_element("css selector", "img.bp-Homecard__Photo--image")
            image_url = image_element.get_attribute("src")
        except:
            image_url = "N/A"
        
        try:
            # Extract Geo-Coordinates (Latitude & Longitude)
            json_script = listing.find_element("css selector", "script[type='application/ld+json']").get_attribute("innerHTML")
            json_data = json.loads(json_script)

            latitude = json_data[0]["geo"]["latitude"]
            longitude = json_data[0]["geo"]["longitude"]
        except:
            latitude = "N/A"
            longitude = "N/A"

        # Store the data
        scraped_data.append({
            "Price": price,
            "Address": address,
            "Beds": beds,
            "Baths": baths,
            "SqFt": sqft,
            "Link": link,
            "Image URL": image_url, 
            "Latitude": latitude,
            "Longitude": longitude
        })

    # Pagination: Check if a "Next Page" button exists
    try:
        next_button = driver.find_element("css selector", "button.PageArrow__direction--next")
        next_button.click()
        page_number += 1
        time.sleep(random.uniform(5, 10))  # Random delay before next request
    except:
        print("No more pages. Scraping complete.")
        break  # Exit loop when no next page exists

# Convert to DataFrame and save as CSV
df = pd.DataFrame(scraped_data)
df.to_csv("data/redfin_hollywood_hills.csv", index=False)

print(f"Scraping complete! {len(df)} listings saved to data/redfin_hollywood_hills.csv")

# Close the browser
driver.quit()
```

## Next Up: Data Cleaning & Analysis

In **Part 2**, we’ll clean and analyze our scraped real estate data, ensuring it’s structured for visualization and further processing.

🔗 **Dive into the next part of our tutorial where we flex our muscles in [data analysis of our real estate data](https://hackr.io/blog/how-to-create-a-python-real-estate-data-pipeline-data-analysis)!**
