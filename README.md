# Python-Script-for-returning-200mresponse-from-bunch-of-URL


import requests
from openpyxl import Workbook

def check_urls(url_file, output_file):
  """
  Checks URLs from a text file and writes status codes and URLs to an Excel file.

  Args:
      url_file (str): Path to the text file containing URLs.
      output_file (str): Path to the Excel file where results will be saved.
  """
  # Create a new workbook
  wb = Workbook()
  ws = wb.active
  ws.append(["URL", "Status Code"])  # Header row

  # Read URLs from the text file
  with open(url_file, 'r') as f:
    for line in f:
      url = line.strip()
      
      # Check if URL is empty or starts with a comment character
      if not url or url.startswith('#'):
        continue

      try:
        response = requests.get(url)
        status_code = response.status_code
      except requests.exceptions.RequestException as e:
        status_code = f"Error: {e}"
      

      if status_code == 200:
        print(f'{url} ................. {status_code}')
        ws.append([url, status_code])

  # Save the workbook
  wb.save(output_file)

# Example usage
url_file = "test_url.txt"
output_file = "url_status.xlsx"
check_urls(url_file, output_file)

print(f"Results written to {output_file}")  
