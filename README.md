# Kalvium-Data-Analyst
Kalvium Data Analyst task
# Lok Sabha Election Results Analysis

This repository contains a data analysis of the recently concluded Lok Sabha election results. The analysis includes scraping the election results data from the Election Commission of India's website, processing the data, and generating insights.

## Table of Contents
- [Introduction](#introduction)
- [Data Scraping](#data-scraping)
- [Installation](#installation)
- [Usage](#usage)
- [Insights](#insights)
  - [Insight 1: Vote Share by Party](#insight-1-vote-share-by-party)
  - [Insight 2: Seat Change Compared to Previous Election](#insight-2-seat-change-compared-to-previous-election)
  - [Insight 3: Voter Turnout](#insight-3-voter-turnout)
  - [Insight 4: Winning Margins](#insight-4-winning-margins)
  - [Insight 5: Performance in Key States](#insight-5-performance-in-key-states)
  - [Insight 6: Gender Representation](#insight-6-gender-representation)
  - [Insight 7: First-time Winners](#insight-7-first-time-winners)
  - [Insight 8: Incumbent Re-elections](#insight-8-incumbent-re-elections)
  - [Insight 9: Closest Contests](#insight-9-closest-contests)
  - [Insight 10: Historical Comparison](#insight-10-historical-comparison)
- [Sources](#sources)
- [Methods](#methods)
- [Key Findings](#key-findings)
- [Visualization](#visualization)

## Introduction

This project aims to provide a comprehensive analysis of the 2024 Lok Sabha election results. The data is scraped from the Election Commission of India's official website and various insights are derived from the processed data.

## Data Scraping

The data is scraped from the Election Commission of India's results page using BeautifulSoup. If the data is loaded dynamically, Selenium is used to ensure all data is captured correctly.

## Installation

1. Clone the repository:
    ```sh
    git clone https://github.com/yourusername/lok-sabha-election-analysis.git
    cd lok-sabha-election-analysis
    ```

2. Install the required packages:
    ```sh
    pip install -r requirements.txt
    ```

3. If using Selenium, ensure you have the appropriate WebDriver for your browser:
    - [ChromeDriver](https://sites.google.com/a/chromium.org/chromedriver/)
    - [GeckoDriver for Firefox](https://github.com/mozilla/geckodriver/releases)

## Usage

To scrape the election results data and save it to a CSV file, run:

```sh
python scrape_election_results.py


### scrape_election_results.py

Here's the complete Python script for scraping the election results and saving them to a CSV file:

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
from selenium import webdriver
from selenium.webdriver.common.by import By
import time

def scrape_with_bs4():
    url = "https://results.eci.gov.in/pcresults.htm"  # Update with the correct URL
    response = requests.get(url)
    soup = BeautifulSoup(response.content, "html.parser")
    table = soup.find("table", {"id": "datatable"})

    if table is None:
        print("Table not found. Here is the HTML:")
        print(soup.prettify())
    else:
        data = []
        headers = [th.text.strip() for th in table.find_all("th")]
        data.append(headers)

        for row in table.find_all("tr")[1:]:
            cols = row.find_all("td")
            cols = [ele.text.strip() for ele in cols]
            data.append(cols)

        df = pd.DataFrame(data[1:], columns=data[0])
        df.to_csv("election_results.csv", index=False)
        print("Data saved to election_results.csv")

def scrape_with_selenium():
    driver_path = 'path/to/chromedriver'  # Change to the path where your chromedriver is located
    driver = webdriver.Chrome(driver_path)
    url = "https://results.eci.gov.in/pcresults.htm"  # Update with the correct URL
    driver.get(url)

    time.sleep(10)  # Adjust the sleep time as needed

    table = driver.find_element(By.ID, "datatable")

    data = []
    headers = [th.text.strip() for th in table.find_elements(By.TAG_NAME, "th")]
    data.append(headers)

    rows = table.find_elements(By.TAG_NAME, "tr")[1:]  # Skip header row
    for row in rows:
        cols = row.find_elements(By.TAG_NAME, "td")
        cols = [ele.text.strip() for ele in cols]
        data.append(cols)

    df = pd.DataFrame(data[1:], columns=data[0])
    df.to_csv("election_results.csv", index=False)
    driver.quit()
    print("Data saved to election_results.csv")

# Uncomment one of the following lines to use the respective scraping method
# scrape_with_bs4()
# scrape_with_selenium()
