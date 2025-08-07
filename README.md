import requests
from bs4 import BeautifulSoup

# Define the target URL (you can change this to any news website with <h2> headlines)
URL = "https://www.bbc.com/news"

# Set a custom User-Agent to mimic a browser
HEADERS = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64)'
}

try:
    # Send GET request to the website
    response = requests.get(URL, headers=HEADERS)
    response.raise_for_status()  # Raise an error for bad status

    # Parse the HTML using BeautifulSoup
    soup = BeautifulSoup(response.text, 'html.parser')

    # Find all h2 tags (common for headlines)
    headlines = soup.find_all('h2')

    # Extract text and clean it
    titles = [headline.text.strip() for headline in headlines if headline.text.strip() != ""]

    # Save to a text file
    with open("headlines.txt", "w", encoding="utf-8") as file:
        for idx, title in enumerate(titles, 1):
            file.write(f"{idx}. {title}\n")

    print("✅ Headlines scraped and saved to headlines.txt successfully!")

except requests.RequestException as req_err:
    print(f"Request error: {req_err}")

except Exception as e:
    print(f"An unexpected error occurred: {e}")
