# Data Scraping

## Website Used
**Yellow Pages**

## Website Information

Yellow Pages is one of the largest business directories in the world, filled with valuable business information. It offers a wide range of details on local companies, services, and goods, making it a great resource for generating leads for marketers. Data that can be scraped from Yellow Pages includes business names, emails, street addresses, ratings and reviews, phone numbers, and more.

## Problem Statement

The goal is to extract and download data such as "title", "address", "number", "reviews", and "ratings" from the Yellow Pages website for categories like "Real Estate Brokers", "Commercial Properties", and "Residential Properties" across different locations in Luzon, Visayas, and Mindanao. This data needs to be compiled into an Excel spreadsheet.

## Method

To scrape the Yellow Pages website, we'll use Python and libraries like `requests`, `pandas`, and `BeautifulSoup`. Here are the steps:

1. **Install Libraries**: Ensure that `requests`, `pandas`, and `BeautifulSoup` are installed.
2. **Import Libraries**: Import the necessary libraries for web scraping and data manipulation.
3. **Initialize DataFrame**: Create a DataFrame to organize the scraped data into rows and columns.
4. **Set Target Website**: Define the target URL for scraping.
5. **Send GET Request**: Use the `requests` library to send a GET request to the target website.
6. **Parse HTML**: Use `BeautifulSoup` to parse the HTML and build an HTML tree from which data can be extracted.
7. **Inspect Webpage**: Inspect the webpage to find where the desired data is stored (usually within specific HTML tags).
8. **Extract Data**: Loop through the HTML elements to extract the desired data (title, address, number, reviews, rating).
9. **Handle Errors**: Use try-except blocks to handle any extraction errors.
10. **Clean Data**: Remove any instances with null values from the DataFrame.
11. **Save Data**: Save the cleaned data to a CSV file.

## Algorithm

We will implement Python libraries such as `pandas` for data manipulation, `requests` for sending HTTP requests, `BeautifulSoup` for parsing HTML and extracting information, and `itertools` for iterating over data structures. The main objective is to extract specific information from Yellow Pages for the categories "Real Estate Brokers", "Commercial Properties", and "Residential Properties" across different locations in Luzon, Visayas, and Mindanao.

## Solution

The web scraping process will operate as follows:

1. **Visit Yellow Pages Website**: Navigate to the website.
2. **Inspect HTML Tags**: Identify where the data is located within the HTML structure by inspecting elements.
3. **Identify Data Locations**: Determine the HTML tags (e.g., `div`, `span`) and classes/IDs that contain the relevant data.
4. **Import Libraries**: Import `requests`, `pandas`, `BeautifulSoup`, and `itertools`.
5. **Initialize DataFrame**: Set up a DataFrame to organize the extracted data.
6. **Send Requests**: Send GET requests to the target URLs for the specified categories and locations.
7. **Extract Data**: Use loops to extract "title", "address", "number", "reviews", and "rating" from the HTML content.
8. **Clean Data**: Remove any rows with null values from the DataFrame.
9. **Save Data**: Export the DataFrame to a CSV file.

### Example Code

```python
import requests
import pandas as pd
from bs4 import BeautifulSoup
from itertools import chain

# Initialize DataFrame
columns = ["Title", "Address", "Number", "Reviews", "Rating"]
df = pd.DataFrame(columns=columns)

# Function to extract data
def extract_data(category, location, page):
    url = f"https://www.yellowpages.com/search?search_terms={category}&geo_location_terms={location}&page={page}"
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')
    
    # Extract data
    for result in soup.find_all('div', class_='result'):
        try:
            title = result.find('a', class_='business-name').text
            address = result.find('div', class_='street-address').text
            number = result.find('div', class_='phones').text
            reviews = result.find('span', class_='count').text
            rating = result.find('div', class_='ratings').find('span')['class'][1].split('-')[-1]
            df.loc[len(df)] = [title, address, number, reviews, rating]
        except Exception as e:
            continue

# Scrape data
categories = ["Real Estate Brokers", "Commercial Properties", "Residential Properties"]
locations = ["Luzon", "Visayas", "Mindanao"]
pages = range(1, 9)

for category, location, page in chain(categories, locations, pages):
    extract_data(category, location, page)

# Clean DataFrame
df.dropna(inplace=True)

# Save to CSV
df.to_csv("yellow_pages_data.csv", index=False)
```

## Evaluation

Web scraping is an effective way to gather large amounts of data from websites automatically. Python, with its extensive library support, is well-suited for this task. By using libraries like `requests`, `BeautifulSoup`, and `pandas`, we can efficiently extract and manipulate data from the Yellow Pages website.

## Data Preparation

Data preparation is crucial in machine learning projects. Web scraping allows us to collect data from websites and convert it from unstructured to structured form. This structured data can then be used for analysis or as input for machine learning models.

## Results & Discussion

Using Python libraries such as `BeautifulSoup`, `requests`, `pandas`, and `itertools`, we successfully scraped data from Yellow Pages, including "title", "address", "number", "reviews", and "rating" for specific categories and locations. `BeautifulSoup` enabled us to parse HTML and extract data, while `requests` facilitated sending HTTP requests. `pandas` helped in data manipulation and cleaning, ensuring the final dataset was structured and ready for use.

By following this method, we can automate the extraction of valuable business information from Yellow Pages and save it in a structured format like CSV, making it accessible for further analysis or lead generation.