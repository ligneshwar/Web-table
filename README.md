## GITHUB LINK:
### https://github.com/ligneshwar/Web-table
# Selenium Web Table Automation Exercises
**Question:**

Write a Selenium script for each test case and verify that the expected result is achieved.

| Test Case | Task | Expected Result |
|---|---|---|
| TC01 | Print all column headings from the web table. | All column headings are displayed. |
| TC02 | Print the first data row from the web table. | The first employee record is displayed. |
| TC03 | Print the last data row from the web table. | The last employee record is displayed. |
| TC04 | Search for an employee using their last name. | The matching employee record is displayed. |
| TC05 | Extract and print all email addresses from the web table. | Every email address is printed. |
| TC06 | Find the employee with the highest **Due** amount. | The employee with the highest Due amount is identified. |
| TC07 | Verify whether a particular website link exists in the web table. | The test reports **PASS** if the link exists or **FAIL** if it does not. |
| TC08 | Count the total number of data rows without counting the header row. | The total number of data rows is displayed correctly. |

## Program:
```python

from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
driver = webdriver.Edge()
wait = WebDriverWait(driver, 10)
driver.get("https://assertqa.com/practice/webtables")
driver.maximize_window()
table = wait.until(
    EC.visibility_of_element_located((By.TAG_NAME, "table"))
)
headers = table.find_elements(By.XPATH, ".//thead/tr/th")
rows = table.find_elements(By.XPATH, ".//tbody/tr")
print("TC01: Column Headings")
for header in headers:
    print(header.text)
print("TC02: First Data Row")
if rows:
    print(rows[0].text)
else:
    print("No data rows found")
print("TC03: Last Data Row")
if rows:
    print(rows[-1].text)
else:
    print("No data rows found")
print("TC04: Search by Last Name")
last_name = input("Enter employee last name: ")
found = False
for row in rows:
    if last_name.lower() in row.text.lower():
        print("Matching record:", row.text)
        found = True
if not found:
    print("No matching employee found")
print("TC05: All Email Addresses")
for row in rows:
    cells = row.find_elements(By.TAG_NAME, "td")
    for cell in cells:
        if "@" in cell.text:
            print(cell.text)

print("TC06: Highest Due Amount")
highest_due = None
highest_due_row = None
for row in rows:
    cells = row.find_elements(By.TAG_NAME, "td")
    for index, header in enumerate(headers):
        if header.text.strip().lower() == "due":
            due_index = index
            break
    else:
        due_index = None
    if due_index is not None and due_index < len(cells):
        due_text = cells[due_index].text.strip()
        due_value = float(
            due_text.replace("$", "").replace(",", "")
        )
        if highest_due is None or due_value > highest_due:
            highest_due = due_value
            highest_due_row = row.text
if highest_due_row is not None:
    print("Employee record:", highest_due_row)
    print("Highest Due amount:", highest_due)
else:
    print("Due column or valid Due values not found")
print("TC07: Verify Website Link")
website = input("Enter website URL or text to search: ")
links = driver.find_elements(By.XPATH, "//a")
link_found = False
for link in links:
    href = link.get_attribute("href") or ""
    text = link.text.strip()
    if website.lower() in href.lower() or website.lower() in text.lower():
        link_found = True
        print("PASS: Website link exists")
        print("Link:", href)
        break
if not link_found:
    print("FAIL: Website link does not exist")
print("\nTC08: Count Data Rows")
print("Total data rows:", len(rows))
driver.quit()

```

## Output:

<img width="1508" height="837" alt="image" src="https://github.com/user-attachments/assets/0eb7d1e2-8090-4e69-861c-8cd2c8db2973" />


