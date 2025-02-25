# Python Automated Real Estate Data Pipeline Project: Part 3 – Dashboard

In [Part 1](https://hackr.io/blog/how-to-create-a-python-real-estate-data-pipeline-web-scraping), we built a real estate web scraper using Selenium, extracting property listings from Redfin, including prices, addresses, bed/bath counts, square footage, and geo-coordinates.

In [Part 2](https://hackr.io/blog/how-to-create-a-python-real-estate-data-pipeline-data-analysis), we cleaned and analyzed the raw scraped data, handling missing values, standardizing formats, and summarizing key real estate trends.

Now, in Part 3, we’ll focus on building an interactive dashboard using Streamlit, allowing users to explore and analyze Hollywood Hills real estate dynamically.

## Part 3: Building an Interactive Real Estate Dashboard

After completing Part 2 of our [Python project](https://hackr.io/blog/python-projects), we successfully cleaned and analyzed real estate listings from Redfin, structuring them into a refined dataset.

Now, in Part 3, we’ll focus on building an interactive dashboard using Streamlit, allowing users to explore and analyze Hollywood Hills real estate dynamically.

What We'll Cover:

- **Loading and preparing data** – Ensuring proper data types and caching for performance.

- **Building a Streamlit dashboard** – Creating a user-friendly interface with filters and visualizations.

- **Adding interactive elements** – Allowing users to filter listings by price, bedrooms, and more.

- **Generating visual insights** – Displaying price distributions and property trends dynamically.

- **Integrating an interactive map** – Displaying property locations using Folium.

- **Preparing for automation** – Laying the groundwork for real-time data tracking in Part 4.

By the end of this tutorial, you'll have a **fully functional real estate dashboard** that dynamically updates based on user input, providing key insights into Hollywood Hills property trends.

## Prerequisites

Don’t worry if you’re not a [Python](https://hackr.io/blog/what-is-python) expert, but before diving into the code, checking that you have these skills under your belt will make this journey smoother and more enjoyable.

- **Basic Python & Pandas knowledge** (e.g., loading and filtering datasets).  
- **Streamlit installed** (`pip install streamlit streamlit-folium`).  
- **Matplotlib & Seaborn installed** (`pip install matplotlib seaborn`).  
- **Completed Part 2** (analyzing and producing a cleaned dataset saved as `redfin_hollywood_hills_cleaned.csv`).

## Required Libraries

We’ll use the following Python packages:

|Library|Purpose|
|---|---|
|`pandas`|Load, clean, and process data|
|`numpy`|Handle numerical transformations and calculations|
|`matplotlib`|Create plots & data visualizations|
|`seaborn`|Enhance statistical plots|
|`streamlit`|Build an interactive web-based dashboard|
|`folium`|Render interactive maps for property locations|
|`streamlit-folium`|Integrate Folium maps into Streamlit|

### Install Dependencies

To install the required libraries, run:

```python
pip install pandas numpy matplotlib seaborn streamlit folium streamlit-folium
```

### Set Up Your Project

Before we start coding, let’s set up the project:

1. Ensure **Python** is installed on your system. If not, download it from the official [Python website](https://www.python.org/downloads/).
    
2. Open your favorite **code editor or IDE** (e.g., VS Code, PyCharm, Jupyter Notebook).
    
3. Create a new Python file, for example, `dashboard.py`.
    
4. Install the required libraries using the `pip install` command above.
    

Now that everything is set up, let's dive head first into our [Python editor](https://app.hackr.io/editors/python) to build this dashboard! 🚀

## Step 1: Load and Prepare the Data

Before we start building the dashboard, we need to load our cleaned dataset and ensure that all numeric columns are correctly formatted.

### Why This Step?

- We use `@st.cache_data` to cache the dataset, improving performance.
- We convert key columns (`Price`, `Beds`, `Baths`, etc.) to numerical types for filtering.
- We remove listings without geo-coordinates, as they cannot be displayed on the map.

```python
import streamlit as st
import pandas as pd
import numpy as np

# Load Data
@st.cache_data
def load_data():
    df = pd.read_csv("data/redfin_hollywood_hills_cleaned.csv")
    
    # Ensure correct data types
    df["Price"] = df["Price"].astype(float)
    df["Beds"] = df["Beds"].astype(float)
    df["Baths"] = df["Baths"].astype(float)
    df["SqFt"] = df["SqFt"].astype(float)
    df["Latitude"] = pd.to_numeric(df["Latitude"], errors="coerce")
    df["Longitude"] = pd.to_numeric(df["Longitude"], errors="coerce")
    
    return df.dropna(subset=["Latitude", "Longitude"])  # Remove entries without coordinates

df = load_data()
```

## Step 2: Creating the Streamlit Dashboard

### Setting Up the Page Layout

This step initializes the Streamlit UI and sets up the main page title and description.

```python
st.title("🏡 Hollywood Hills Real Estate Dashboard")
st.write("Analyze real estate trends in Hollywood Hills using interactive visualizations.")
```

### Adding Sidebar Filters

Filters allow users to refine listings based on **price, bedrooms, bathrooms, and square footage**, helping them find relevant properties faster.

Here's what we'll end up with:  
![Dynamic filters for real estate data](https://cdn.hackr.io/uploads/posts/attachments/173842145588LnJWndsY.webp)Why This Step?

- **User Customization:** Enables users to tailor their search based on their preferences.
    
- **Dynamic Filtering:** Ensures only relevant listings are displayed, improving usability.
    

```python
# Show all properties button
show_all = st.sidebar.checkbox("Show All Properties", value=False)

if show_all:
    filtered_df = df  # Show all properties
else:
    # Determine the max price dynamically and round up to the next 1 million
    min_price_value = max(1000, df["Price"].min())  # Ensure minimum is at least 1,000
    max_price_value = np.ceil(df["Price"].max() / 1_000_000) * 1_000_000  # Round up to next 1M

    # Price Slider
    min_price, max_price = st.sidebar.slider(
        "Select Price Range ($)", 
        min_value=int(min_price_value), 
        max_value=int(max_price_value), 
        value=(int(min_price_value), int(max_price_value)),
        format="$%d"
    )

    # Bedrooms: Round up to the next multiple of 5
    min_beds = 1
    max_beds = int(np.ceil(df["Beds"].max() / 5) * 5)

    # Bathrooms: Round up to the next multiple of 5
    min_baths = 1
    max_baths = int(np.ceil(df["Baths"].max() / 5) * 5)

    # Square Footage: Ensure a sensible range
    min_sqft = 100
    max_sqft = int(np.ceil(df["SqFt"].max() / 10_000) * 10_000)

    # Filters
    selected_beds = st.sidebar.slider("Bedrooms", min_beds, max_beds, (1, 5))
    selected_baths = st.sidebar.slider("Bathrooms", min_baths, max_baths, (1, 5))
    selected_sqft = st.sidebar.slider("Square Footage", min_sqft, max_sqft, (500, 5000))

    # Apply Filters
    filtered_df = df[
        (df["Price"] >= min_price) & (df["Price"] <= max_price) &
        (df["Beds"] >= selected_beds[0]) & (df["Beds"] <= selected_beds[1]) &
        (df["Baths"] >= selected_baths[0]) & (df["Baths"] <= selected_baths[1]) &
        (df["SqFt"] >= selected_sqft[0]) & (df["SqFt"] <= selected_sqft[1])
    ]
```

### Explanation of Dynamic Filters:

- **Price Filter:** The minimum price is at least $1,000, and the maximum is dynamically rounded up to the nearest million.
    
- **Bedrooms & Bathrooms:** The maximum values are rounded up to the next multiple of 5 to provide a cleaner user experience.
    
- **Square Footage:** The maximum value is rounded up to the next multiple of 10,000 for better filtering.
    
- **Dynamic Filtering:** Only listings that match the selected filters are displayed in `filtered_df`.
    

Now that we have **dynamic filtering**, let’s move on to **visualizing real estate trends!** 📊

## 📊 Step 3: Visualizing the Data

### Displaying a Dynamic Table

This table dynamically updates based on user-selected filters and provides an easy way to explore property listings. Check out the code and the output below.

```python
st.subheader(f"📊 {len(filtered_df)} Listings Found")

selected_rows = st.data_editor(
    filtered_df[["Price", "Beds", "Baths", "SqFt", "Address", "Link"]]
    .sort_values(by="Price", ascending=False)
    .reset_index(drop=True),
    use_container_width=True,
    height=400,
    num_rows="dynamic",
    hide_index=True,
    column_config={"Link": st.column_config.LinkColumn()},
    key="table_selection"
)
```

### ![Table of filtered properties for real estate dashboard](https://cdn.hackr.io/uploads/posts/attachments/1738421529xSG3KuPvug.webp)Plotting Price Distribution

Understanding price distribution helps reveal market trends, including **price concentration** and **outlier detection**.

Continuing on from our work in part 2, we'll be using Seaborn to generate plots that we can then display within our dashboard.

```python
import matplotlib.pyplot as plt
import seaborn as sns
import matplotlib.ticker as mtick

st.subheader("💰 Price Distribution")
with st.container():
    fig, ax = plt.subplots(figsize=(8, 4))
    sns.histplot(filtered_df["Price"], bins=30, kde=True, ax=ax)
    ax.xaxis.set_major_formatter(mtick.FuncFormatter(lambda x, _: f'${x/1_000_000:.0f}M'))
    ax.set_xlabel("Price ($)")
    ax.set_ylabel("Count")
    st.pyplot(fig)
```

Explanation:

- **Seaborn’s** `**histplot**`: Creates a histogram with `kde=True` to overlay a smooth density curve.
    
- **Matplotlib Tick Formatter**: Formats x-axis values to display prices in millions (e.g., `$1M`).
    
- **Dynamic Updates**: Updates automatically based on the filtered dataset.
    

Here's what we should see in our dashboard:

### ![Price distribution of filtered properties in real estate dashboard](https://cdn.hackr.io/uploads/posts/attachments/17384216422QL3h1UjTy.webp)Bedrooms & 🛁 Bathrooms Distribution

Examining the distribution of bedrooms and bathrooms helps **identify common home types** in the dataset.

```python
st.subheader("🛏️ Bedrooms & 🛁 Bathrooms Distribution")
with st.container():
    fig, ax = plt.subplots(1, 2, figsize=(12, 5))
    sns.countplot(x="Beds", data=filtered_df, ax=ax[0], hue="Beds", palette="Blues", legend=False)
    ax[0].set_title("Number of Bedrooms")
    ax[0].set_xlabel("Beds")
    sns.countplot(x="Baths", data=filtered_df, ax=ax[1], hue="Baths", palette="Reds", legend=False)
    ax[1].set_title("Number of Bathrooms")
    ax[1].set_xlabel("Baths")
    st.pyplot(fig)
```

Explanation:

- `**countplot**` **for Bedrooms/Bathrooms**: Displays frequency of different bedroom and bathroom counts.
    
- **Color-Coding**: Uses separate color palettes (`Blues` for bedrooms, `Reds` for bathrooms) for clarity.
    

And here's what we should see when we produce both of these plots in our dashboard:

![Distribution of beds and baths in real estate dashbaord](https://cdn.hackr.io/uploads/posts/attachments/1738421695aQz0KDCsVl.webp)Now that we’ve added key visualizations, let’s move on to **integrating an interactive map!**

## Step 4: Interactive Map

### Mapping Property Locations

To enhance the user experience, we will integrate an **interactive map** using `folium`, allowing users to explore real estate listings geographically.

Why Use a Map?

- **Location Matters:** Users can visually inspect the locations of properties in Hollywood Hills.
    
- **Improved Search Experience:** Filtering listings geographically helps identify ideal neighborhoods.
    
- **Enhanced Data Interaction:** Users can click on a property marker to get details such as price, size, and a direct link to the listing.
    

Here's what we should see after coding this feature:

![Map of filtered properties with red pins for selected and blue pins for all other properties](https://cdn.hackr.io/uploads/posts/attachments/1738421738pfSNgiMPaO.webp)Now, let's dive into the code for this interactive feature:

```python
import folium
from streamlit_folium import st_folium

# Get selected address (if any)
selected_address = selected_rows.iloc[0]["Address"] if not selected_rows.empty else None

# Map Visualization
st.subheader("📍 Property Locations")

# Create map centered on Hollywood Hills
m = folium.Map(location=[34.1, -118.3], zoom_start=12)

# Add markers for all filtered properties
for _, row in filtered_df.iterrows():
    popup_info = f"""
    <b>{row['Address']}</b><br>
    Price: ${row['Price']:,.0f}<br>
    Beds: {row['Beds']}, Baths: {row['Baths']}<br>
    SqFt: {row['SqFt']:,}<br>
    <a href="{row['Link']}" target="_blank">View Listing</a>
    """

    # Highlight the selected property in red, others in blue
    icon_color = "red" if selected_address and row["Address"] == selected_address else "blue"

    folium.Marker(
        location=[row["Latitude"], row["Longitude"]],
        popup=popup_info,
        icon=folium.Icon(color=icon_color, icon="home"),
    ).add_to(m)

# Display map
st_folium(m, width=800, height=500)

st.write("Data Source: Redfin Scraper")
```

How this works:

- **Base Map:** The map is created using `folium.Map()`, centered on **Hollywood Hills** with a reasonable zoom level.
    
- **Dynamic Markers:** The `folium.Marker()` function adds **interactive property markers** for each listing, using **latitude and longitude** from our dataset.
    
- **Property Details in Popups:** Each marker has a **popup window** showing:
    
    - Address
        
    - Price (formatted for readability)
        
    - Number of beds/baths
        
    - Square footage
        
    - A clickable link to the listing
        
- **Highlighting Selected Properties:**
    
    - If a **user selects a property from the table**, it is highlighted in **red**.
        
    - Other listings remain in **blue**.
        
- **Interactive Map Integration:** `st_folium()` seamlessly integrates the Folium map into Streamlit, ensuring a responsive user experience.
    

### Key Takeaways

- Provides a geographic overview of property listings.

- Allows users to quickly locate and inspect homes of interest.

- Enhances user engagement with an interactive and visually appealing map.

## Step 5: Running the Dashboard

Now that we have built our **interactive real estate dashboard**, it's time to run the script and see it in action.

To start the dashboard, run the following command in your terminal or command prompt:

```python
streamlit run dashboard.py
```

### What This Does:

- **Launches the Streamlit server** and opens the app in your default browser.
    
- **Loads and displays the cleaned dataset** from Part 2.
    
- **Applies filters** to refine listings based on user input.
    
- **Generates real-time visualizations** of price distribution and property trends.
    
- **Displays an interactive map** to explore listings geographically.
    

Here's what we'd see after running our dashboard:

![Full real-estate dashboard after running the application](https://cdn.hackr.io/uploads/posts/attachments/1738421836DOr5TwDZbS.webp)**Final Thoughts & Next Steps**

- We have successfully built an interactive real estate dashboard with Streamlit.

- The dashboard allows users to **filter, analyze, and visualize** Hollywood Hills property data dynamically.

- The next step is **automating real-time updates** and integrating **historical price tracking** in Part 4!

## Full Program Source Code

```python
import streamlit as st
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.ticker as mtick
import seaborn as sns
import folium
from streamlit_folium import st_folium
import numpy as np

# Load Data
@st.cache_data
def load_data():
    df = pd.read_csv("data/redfin_hollywood_hills_cleaned.csv")
    # Ensure correct data types
    df["Price"] = df["Price"].astype(float)
    df["Beds"] = df["Beds"].astype(float)
    df["Baths"] = df["Baths"].astype(float)
    df["SqFt"] = df["SqFt"].astype(float)
    df["Latitude"] = pd.to_numeric(df["Latitude"], errors="coerce")
    df["Longitude"] = pd.to_numeric(df["Longitude"], errors="coerce")
    return df.dropna(subset=["Latitude", "Longitude"])  # Remove entries without coordinates

df = load_data()

# Streamlit UI
st.title("🏡 Hollywood Hills Real Estate Dashboard")
st.write("Analyze real estate trends in Hollywood Hills using interactive visualizations.")

# Sidebar Filters
st.sidebar.header("🔍 Filter Listings")

# Show all properties button
show_all = st.sidebar.checkbox("Show All Properties", value=False)

if show_all:
    filtered_df = df  # Show all properties
else:
    # Determine the max price dynamically and round up to the next 1 million
    min_price_value = max(1000, df["Price"].min())  # Ensure minimum is at least 1,000
    max_price_value = np.ceil(df["Price"].max() / 1_000_000) * 1_000_000  # Round up to next 1M

    # Price Slider
    min_price, max_price = st.sidebar.slider(
        "Select Price Range ($)", 
        min_value=int(min_price_value), 
        max_value=int(max_price_value), 
        value=(int(min_price_value), int(max_price_value)),
        format="$%d"
    )

    # Bedrooms: Round up to the next multiple of 5
    min_beds = 1
    max_beds = int(np.ceil(df["Beds"].max() / 5) * 5)

    # Bathrooms: Round up to the next multiple of 5
    min_baths = 1
    max_baths = int(np.ceil(df["Baths"].max() / 5) * 5)

    # Square Footage: Ensure a sensible range
    min_sqft = 100
    max_sqft = int(np.ceil(df["SqFt"].max() / 10_000) * 10_000)

    # Filters
    selected_beds = st.sidebar.slider("Bedrooms", min_beds, max_beds, (1, 5))
    selected_baths = st.sidebar.slider("Bathrooms", min_baths, max_baths, (1, 5))
    selected_sqft = st.sidebar.slider("Square Footage", min_sqft, max_sqft, (500, 5000))

    # Apply Filters
    filtered_df = df[
        (df["Price"] >= min_price) & (df["Price"] <= max_price) &
        (df["Beds"] >= selected_beds[0]) & (df["Beds"] <= selected_beds[1]) &
        (df["Baths"] >= selected_baths[0]) & (df["Baths"] <= selected_baths[1]) &
        (df["SqFt"] >= selected_sqft[0]) & (df["SqFt"] <= selected_sqft[1])
    ]

# Display Data Table with Interactive Selection
st.subheader(f"📊 {len(filtered_df)} Listings Found")

# Create an interactive table where users can select a row
selected_rows = st.data_editor(
    filtered_df[["Price", "Beds", "Baths", "SqFt", "Address", "Link"]]
    .sort_values(by="Price", ascending=False)
    .reset_index(drop=True),
    use_container_width=True,  # Makes the table responsive
    height=400,
    num_rows="dynamic",
    hide_index=True,  # Hides the index column
    column_config={"Link": st.column_config.LinkColumn()},  # Make links clickable
    key="table_selection"
)

# Get selected address (if any)
selected_address = selected_rows.iloc[0]["Address"] if not selected_rows.empty else None

# Price Distribution
st.subheader("💰 Price Distribution")
with st.container():
    fig, ax = plt.subplots(figsize=(8, 4))
    
    # Plot histogram
    sns.histplot(filtered_df["Price"], bins=30, kde=True, ax=ax)

    # Format x-axis: Convert to millions and append "M"
    ax.xaxis.set_major_formatter(mtick.FuncFormatter(lambda x, _: f'${x/1_000_000:.0f}M'))

    ax.set_xlabel("Price ($)")
    ax.set_ylabel("Count")
    st.pyplot(fig)

# Beds/Baths Analysis
st.subheader("🛏️ Bedrooms & 🛁 Bathrooms Distribution")
with st.container():
    fig, ax = plt.subplots(1, 2, figsize=(12, 5))

    sns.countplot(x="Beds", data=filtered_df, ax=ax[0], hue="Beds", palette="Blues", legend=False)
    ax[0].set_title("Number of Bedrooms")
    ax[0].set_xlabel("Beds")

    sns.countplot(x="Baths", data=filtered_df, ax=ax[1], hue="Baths", palette="Reds", legend=False)
    ax[1].set_title("Number of Bathrooms")
    ax[1].set_xlabel("Baths")

    st.pyplot(fig)

# Map Visualization
st.subheader("📍 Property Locations")

# Create map centered on Hollywood Hills
m = folium.Map(location=[34.1, -118.3], zoom_start=12)

# Add markers for all filtered properties
for _, row in filtered_df.iterrows():
    popup_info = f"""
    <b>{row['Address']}</b><br>
    Price: ${row['Price']:,.0f}<br>
    Beds: {row['Beds']}, Baths: {row['Baths']}<br>
    SqFt: {row['SqFt']:,}<br>
    <a href="{row['Link']}" target="_blank">View Listing</a>
    """

    # Check if the row matches the selected property
    icon_color = "red" if selected_address and row["Address"] == selected_address else "blue"

    folium.Marker(
        location=[row["Latitude"], row["Longitude"]],
        popup=popup_info,
        icon=folium.Icon(color=icon_color, icon="home"),
    ).add_to(m)

# Display map with full width
st_folium(m, width=800, height=500)

st.write("Data Source: Redfin Scraper")
```

## Next Up: Automating & Expanding the Dashboard

We’ve built a fully interactive **real estate dashboard** that allows users to explore Hollywood Hills listings dynamically. In **Part 4**, we’ll **automate** the data pipeline and expand the dashboard with historical price tracking!

🔗 Dive into [part 4](https://hackr.io/blog/how-to-create-a-python-real-estate-data-pipeline-automation) to enhance the entire pipeline with automation, logging, and historical analyses.