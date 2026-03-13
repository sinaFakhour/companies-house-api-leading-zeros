Companies House API – Handling Leading Zeros in Company Numbers


## The Problem##

While extracting company data from the Companies House API, I noticed that many API responses were returning blank results, even though the companies clearly had data when searched manually on the Companies House website.
This was confusing because the same company numbers worked perfectly when searched manually.


## The Discovery

After investigating the API responses, I discovered that the issue was related to company numbers with leading zeros.
When working with datasets stored in CSV or Excel files, leading zeros are often automatically removed.

**Example:**

00258527 → 258527

The Companies House website interface automatically restores missing leading zeros, so manual searches still work. However, the API requires the company number to be exactly 8 digits and does not automatically add the missing zeros.


## The Fix

Before sending requests to the API, company numbers should be normalized to ensure they always contain 8 digits.

**Python:**
```python
if len(company_number) < 8 and company_number[0].isdigit():
    company_number = company_number.zfill(8)
```

This simple fix pads missing leading zeros and ensures the API request is correctly formatted.


## The Lesson

Manual testing through a website interface can sometimes be misleading because web interfaces often perform automatic formatting or corrections that APIs do not.
When working with APIs, it is important to verify the exact input format required by the API documentation and ensure the data is normalized before sending requests.
