# Python Automated Real Estate Data Pipeline Project: Part 2 – Data Analysis

In [Part 1](https://hackr.io/blog/how-to-create-a-python-real-estate-data-pipeline-web-scraping), we built a real estate web scraper using Selenium, extracting property listings from Redfin, including prices, addresses, bed/bath counts, square footage, and geo-coordinates.

However, raw scraped data is often messy — it may contain missing values, inconsistencies, and formatting issues. Before we can analyze or visualize trends, we need to clean and structure our dataset.

## Part 2: Analyzing Scraped Real Estate Data

After completing part 1 of our [Python project](https://hackr.io/blog/python-projects), we successfully scraped real estate listings from Redfin using Selenium and stored them in a structured dataset. Now, in Part 2, we’ll focus on cleaning, exploring, and analyzing the data to uncover key insights about the Hollywood Hills real estate market.

We’ll cover:  
- **Data Cleaning** – Handling missing values, formatting columns, and filtering invalid data  
- **Exploratory Data Analysis (EDA)** – Summarizing trends in prices, property sizes, and other attributes  
- **Visualizations** – Plotting price distributions, bedroom/bathroom trends, and identifying high/low-end properties  
- **Saving the Cleaned Dataset** – Preparing our data for further analysis or interactive dashboards

## Why Clean & Analyze Data?

Before we can move forward with analysis and visualization, we need to:

**- Handle missing values** (e.g., missing prices, addresses, or geo-coordinates).  
- **Standardize formats** (e.g., remove `$` and `,` from prices for numerical analysis).  
- **Extract useful insights** (e.g., highest/lowest-priced listings, price distribution).  
- **Prepare the dataset for visualization** in **Part 3** (interactive dashboard).

## Prerequisites

Don’t worry if you’re not a [Python](https://hackr.io/blog/what-is-python) expert, but before diving into the code, checking that you have these skills under your belt will make this journey smoother and more enjoyable.

- **Basic Python & Pandas knowledge** (e.g., loading data, working with DataFrames).

- **Completed Part 1** ([scraping real estate data with Selenium](https://hackr.io/blog/how-to-create-a-python-real-estate-data-pipeline-web-scraping)) - if you haven't done this, go back to Part 1.

- **Matplotlib & Seaborn installed** for data visualization.

## Required Libraries

We’ll use the following Python packages:

|Library|Purpose|
|---|---|
|`pandas`|Load, clean, and process data|
|`numpy`|Handle missing values & numeric transformations|
|`matplotlib`|Create plots & data visualizations|
|`seaborn`|Enhance statistical plots|
|`re`|Extract numeric values from text fields|

### Install Dependencies

```python
pip install pandas numpy matplotlib seaborn
```

### Set up Your Project

Before we start coding, let’s set up the project:

1. Make sure Python is installed on your computer. If not, download it from the [official Python website](https://www.python.org/).  
2. Open your favorite code editor or IDE.  
3. Create a new Python file, for example, _analyze.py_.

Great, now, let's dive head first into our [Python editor](https://app.hackr.io/editors/python) to get this build started.

## Step 1: Load & Inspect the Dataset

Let’s start by loading the dataset we scraped in **Part 1** by [reading the CSV with Pandas](https://hackr.io/blog/pandas-read-csv-function).`   `

```python
import pandas as pd

# Load the dataset
df = pd.read_csv("data/redfin_hollywood_hills.csv")

# Display basic info
def inspect_data(df):
    print("\nData Overview:")
    print(df.info())  # Column types, missing values
    print("\nFirst Few Rows:")
    print(df.head())

inspect_data(df)
```

### What Are We Checking?

- **Column types** (e.g., ensure numeric columns are actually numeric).
- **Missing values** (e.g., is any property missing its price, beds, or location?).
- **Data format issues** (e.g., prices stored as text due to `$` and `,`).

We can then use this function to print a summary of the dataset, including:

- The number of rows and columns.

- Data types of each column.

- Missing values.

- The first few rows of the dataset for quick inspection.

## Step 2: Handling Missing Data

Web scraping often results in **incomplete datasets** due to missing or malformed values. We need to:

- **Replacing invalid values** (`—`, `N/A`, empty strings) with `NaN`.  
- **Drop rows** where key attributes like price, beds, baths, and square footage are missing.  
- **Reset the index** after dropping rows.

```python
import numpy as np

def clean_data(df):
    print("\nHandling Missing Data...")
    
    # Replace invalid values with NaN
    df.replace({'—': np.nan, 'N/A': np.nan, '': np.nan}, inplace=True)
    
    # Drop rows with missing essential values
    df.dropna(subset=["Price", "Beds", "Baths", "SqFt"], inplace=True)
    
    # Reset index
    df.reset_index(drop=True, inplace=True)
    
    print(f"Cleaned Data: {len(df)} rows remaining.")
    return df

df = clean_data(df)
```

Now, our dataset is **cleaner and more reliable** for analysis, but we can go a step further.

Let's also handle rows where we're missing geo-location data. In the simplest case, we could drop these from our data set, as without these, we cannot visualize the properties on a map inside our future dashboard.

```python
def handle_missing_geo(df):
    missing_geo = df[df['Latitude'].isna() | df['Longitude'].isna()]
    if not missing_geo.empty:
        print(f"\nWarning: {len(missing_geo)} listings are missing geo-coordinates.")
        df = df.dropna(subset=['Latitude', 'Longitude'])  # Drop listings without location data
        print(f"{len(df)} listings retained after removing missing geo-coordinates.")
    return df
```

## Step 3: Formatting Data

Scraped data is often stored as text. We need to **convert numerical columns to the correct types**:

- Convert `Price`, `Beds`, `Baths`, and `SqFt` to numeric values.  
- Strip unnecessary characters from numerical columns.  
- Extract numeric values from text-based columns (e.g., `"2 beds"` → `2`).

```python
import re

def extract_numeric(value):
    """Extracts numeric values from a string. Handles cases like '1 bed', '2 baths', 'Studio'."""
    if isinstance(value, str):
        match = re.search(r'\d+', value)  # Find first numeric value
        return float(match.group()) if match else np.nan
    return np.nan

def convert_columns(df):
    print("Converting Columns...")
    
    # Strip whitespace from all columns
    df = df.applymap(lambda x: x.strip() if isinstance(x, str) else x)
    
    # Convert Price column: Remove "$" and ",", then convert to float
    df['Price'] = df['Price'].str.replace('[$,]', '', regex=True).replace('', np.nan).astype(float)
    
    # Convert SqFt column
    df['SqFt'] = df['SqFt'].replace(['—', '', 'N/A'], np.nan)
    df['SqFt'] = df['SqFt'].str.replace(',', '', regex=True)
    df['SqFt'] = pd.to_numeric(df['SqFt'], errors='coerce')
    
    # Convert Beds & Baths
    df['Beds'] = df['Beds'].apply(extract_numeric)
    df['Baths'] = df['Baths'].apply(extract_numeric)
    
    # Convert Latitude & Longitude to float
    df['Latitude'] = pd.to_numeric(df['Latitude'], errors='coerce')
    df['Longitude'] = pd.to_numeric(df['Longitude'], errors='coerce')
    
    print("Data Cleaning Complete!")
    return df

df = convert_columns(df)
```

## Step 4: Data Analysis & Visualization

### Summary Statistics

Now that our data is clean, we can **summarize key statistics**:

```python
def summarize_data(df):
    print("\nSummary Statistics:")
    print(df.describe())

summarize_data(df)
```

We should see something like the following:  
![Generating summary statistics for real estate data with Pandas](https://cdn.hackr.io/uploads/posts/attachments/1738583796PnB3C27jjO.webp)

## Step 5: Visualizing Trends

Now that we’ve gathered and cleaned our real estate data, it’s time to **visualize key trends** to better understand the market. In this step, we’ll use **Matplotlib and Seaborn** to create insightful plots that reveal patterns in pricing, bedroom and bathroom distribution, and extreme listings.

### Price Distribution

Understanding the distribution of listing prices helps us identify whether the market is **skewed** (e.g., a few extremely high-priced properties inflating the average) or **normally distributed** (most homes clustering around a typical price range).

```python
import matplotlib.pyplot as plt
import seaborn as sns

def plot_price_distribution(df):
    plt.figure(figsize=(10,5))
    sns.histplot(df['Price'], bins=30, kde=True)
    plt.xlabel("Price (in millions)")
    plt.ylabel("Count")
    plt.title("Price Distribution of Listings")
    plt.show()

plot_price_distribution(df)
```

- We use Seaborn to create a histogram, which shows the frequency of different price ranges.  
- The `kde=True` parameter overlays a **Kernel Density Estimate (KDE)** curve, giving a smooth approximation of the distribution.

Here's an example of what we might see:  
![Generating plots of price distribution for real estate data with Seaborn](https://cdn.hackr.io/uploads/posts/attachments/1738583667naWryFinhR.webp)

### Bedroom & Bathroom Distribution

This analysis helps us understand typical property sizes in the dataset.

```python
def analyze_beds_baths(df):
    plt.figure(figsize=(10,5))
    sns.countplot(x='Beds', data=df, order=sorted(df['Beds'].dropna().unique()))
    plt.xlabel("Number of Bedrooms")
    plt.ylabel("Count")
    plt.title("Distribution of Bedrooms in Listings")
    plt.show()
    
    plt.figure(figsize=(10,5))
    sns.countplot(x='Baths', data=df, order=sorted(df['Baths'].dropna().unique()))
    plt.xlabel("Number of Bathrooms")
    plt.ylabel("Count")
    plt.title("Distribution of Bathrooms in Listings")
    plt.show()

analyze_beds_baths(df)
```

- We use **Seaborn's `countplot`** to show the **frequency of listings** for each number of bedrooms and bathrooms.

Here's an example of what we might see:  
![Generating plots of bedroom distribution for real estate data with Seaborn](https://cdn.hackr.io/uploads/posts/attachments/1738583696o19GWfVD4U.webp)

![Generating plots of bathroom distribution for real estate data with Seaborn](https://cdn.hackr.io/uploads/posts/attachments/1738583715PKX01eak4X.webp)

### Top & Bottom Listings

It’s always interesting to look at the **most expensive and cheapest** listings to get a sense of market extremes.

```python
def show_extreme_listings(df):
    print("\nTop 5 Most Expensive Listings:")
    print(df.nlargest(5, 'Price'))
    print("\nTop 5 Cheapest Listings:")
    print(df.nsmallest(5, 'Price'))
```

Here's an example of what we might expect to see:  
![Generating summary data for extreme listings for real estate data with Pandas](https://cdn.hackr.io/uploads/posts/attachments/1738583746GoWrX5hS79.webp)

### Key Takeaways

- **Visualizing price distributions** helps identify market trends (e.g., luxury vs. affordable housing).  
- **Understanding bedroom & bathroom counts** highlights the most common property types.  
- **Identifying extreme listings** gives insights into the highest- and lowest-priced properties.

This step helps us uncover **hidden insights** before we move on to building a **Streamlit dashboard** for interactive exploration!

## Step 6: Run Script & Save Cleaned Data

Now that we've **cleaned, transformed, and analyzed** the real estate dataset, it’s time to **execute the script** and save the final version as a CSV file. This step ensures that our dataset is ready for further analysis or dashboard visualization.

```python
if __name__ == "__main__":
    inspect_data(df)
    df = clean_data(df)
    df = convert_columns(df)
    df = handle_missing_geo(df)
    summarize_data(df)
    plot_price_distribution(df)
    analyze_beds_baths(df)
    show_extreme_listings(df)
    
    # Save the cleaned dataset
    df.to_csv("data/redfin_hollywood_hills_cleaned.csv", index=False)
    print("Cleaned data saved as 'redfin_hollywood_hills_cleaned.csv'.")
```

**Final Thoughts & Next Steps**

- We have successfully **cleaned, structured, and visualized** real estate data from **Hollywood Hills listings**.  
- The cleaned dataset is now stored as **`redfin_hollywood_hills_cleaned.csv`** and ready for **further analysis**.  
- The next step is to build an **interactive dashboard** using **Streamlit** to explore the data dynamically!

## Full Program Source Code

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
import re

# Load the dataset
df = pd.read_csv("data/redfin_hollywood_hills.csv")

# Display basic info about the dataset
def inspect_data(df):
    print("\nData Overview:")
    print(df.info())
    print("\nFirst Few Rows:")
    print(df.head())

# Handle missing data
def clean_data(df):
    print("\nHandling Missing Data...")
    
    # Replace invalid values
    df.replace({'—': np.nan, 'N/A': np.nan, '': np.nan}, inplace=True)

    # Drop rows with missing essential values (Price, Beds, Baths, SqFt)
    df.dropna(subset=["Price", "Beds", "Baths", "SqFt"], inplace=True)
    
    # Reset index
    df.reset_index(drop=True, inplace=True)
    
    print(f"Cleaned Data: {len(df)} rows remaining.")
    return df

def extract_numeric(value):
    """ Extracts numeric values from a string. Handles cases like '1 bed', '2 baths', 'Studio' """
    if isinstance(value, str):
        match = re.search(r'\d+', value)  # Find first numeric value
        return float(match.group()) if match else np.nan  # Convert to float, otherwise NaN
    return np.nan  # Return NaN for non-string values

def convert_columns(df):
    print("Converting Columns...")
    
    # Strip whitespace from all columns
    df = df.applymap(lambda x: x.strip() if isinstance(x, str) else x)
    
    # Convert Price column: Remove "$" and "," then convert to float
    df['Price'] = df['Price'].str.replace('[$,]', '', regex=True).replace('', np.nan).astype(float)
    
    # Convert SqFt column: Replace non-numeric characters, handle empty values
    df['SqFt'] = df['SqFt'].replace(['—', '', 'N/A'], np.nan)  # Handle missing values
    df['SqFt'] = df['SqFt'].str.replace(',', '', regex=True)  # Remove commas
    df['SqFt'] = pd.to_numeric(df['SqFt'], errors='coerce')  # Convert to float safely
    
    # Convert Beds & Baths: Extract numbers from text and convert to float
    df['Beds'] = df['Beds'].apply(extract_numeric)
    df['Baths'] = df['Baths'].apply(extract_numeric)

    # Convert Latitude & Longitude to float (handle missing values)
    df['Latitude'] = pd.to_numeric(df['Latitude'], errors='coerce')
    df['Longitude'] = pd.to_numeric(df['Longitude'], errors='coerce')

    print("Data Cleaning Complete!")
    return df

# Generate summary statistics
def summarize_data(df):
    print("\nSummary Statistics:")
    print(df.describe())

# Plot price distribution
def plot_price_distribution(df):
    plt.figure(figsize=(10,5))
    sns.histplot(df['Price'], bins=30, kde=True)
    plt.xlabel("Price (in millions)")
    plt.ylabel("Count")
    plt.title("Price Distribution of Listings")
    plt.show()

# Analyze Beds/Baths
def analyze_beds_baths(df):
    plt.figure(figsize=(10,5))
    sns.countplot(x='Beds', data=df, order=sorted(df['Beds'].dropna().unique()))
    plt.xlabel("Number of Bedrooms")
    plt.ylabel("Count")
    plt.title("Distribution of Bedrooms in Listings")
    plt.show()
    
    plt.figure(figsize=(10,5))
    sns.countplot(x='Baths', data=df, order=sorted(df['Baths'].dropna().unique()))
    plt.xlabel("Number of Bathrooms")
    plt.ylabel("Count")
    plt.title("Distribution of Bathrooms in Listings")
    plt.show()

# Handle missing geo-coordinates
def handle_missing_geo(df):
    missing_geo = df[df['Latitude'].isna() | df['Longitude'].isna()]
    if not missing_geo.empty:
        print(f"\nWarning: {len(missing_geo)} listings are missing geo-coordinates.")
        df = df.dropna(subset=['Latitude', 'Longitude'])  # Drop listings without location data
        print(f"{len(df)} listings retained after removing missing geo-coordinates.")
    return df

# Top & Bottom Listings
def show_extreme_listings(df):
    print("\nTop 5 Most Expensive Listings:")
    print(df.nlargest(5, 'Price'))
    print("\nTop 5 Cheapest Listings:")
    print(df.nsmallest(5, 'Price'))

# Run analysis
if __name__ == "__main__":
    inspect_data(df)
    df = clean_data(df)
    df = convert_columns(df)
    df = handle_missing_geo(df)
    summarize_data(df)
    plot_price_distribution(df)
    analyze_beds_baths(df)
    show_extreme_listings(df)
    
    # Save the cleaned dataset
    df.to_csv("data/redfin_hollywood_hills_cleaned.csv", index=False)
    print("Cleaned data saved as 'redfin_hollywood_hills_cleaned.csv'.")
```

## Next Up: Building a Streamlit Dashboard

In **Part 3**, we’ll **build an interactive dashboard** with **Streamlit**, enabling users to **filter, search, and visualize listings** dynamically.

🔗 **Dive into the next part of our tutorial where we tie it all together to [create an interactive dashboard in Streamlit](https://hackr.io/blog/how-to-create-a-python-real-estate-data-pipeline-dashboard)!**