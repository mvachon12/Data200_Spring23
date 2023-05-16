# Data 200: Database Systems and Data Management for Data Analytics


# Name: Maia Vachon

# Take Home Final Exam
<font color='red'>**Due Date:** May 16, 12pm (noon) </font>
---

Task: Scrape data from a job search website


Website: Indeed.com

Objective: Collect job postings that match certain keywords and location filters, and then perform data analysis to extract insights about the job market.

Instructions:

1. Choose a location and a set of keywords that are relevant to your field of study. For example, you can schoose a field such as data science, computer science, or any other field you are interested in. In terms of the location, you might choose to search for "data analytics" jobs in "San Francisco". You are free to choose the field and location on this assignment.
2. Scrape job postings from Indeed.com using selenium and python. Your code should extract the following information for each job posting:
- Job title
- Company name
- Job description
- Job location
- Date posted
3. Clean and preprocess the data as necessary. You may want to remove duplicates and perform other data cleaning tasks to prepare the data for analysis.
4. Use data analysis techniques to extract insights about the job market. For example, you might:
- Identify which companies are hiring the most for the given job titles and locations.
- Determine the distribution of job titles and their average salaries.
- Analyze the frequency of certain keywords in job descriptions, and determine which skills and qualifications are most in demand.
5. Write a report summarizing your findings. Your report should include tables and visualizations to support your conclusions, and should provide actionable insights that could be used by job seekers, employers, or policymakers.
Submit your code and report as a single .ipynb file (you can do it in this current notebook as a combination of code cells and markdown cells), along with any necessary instructions for running your code. Make sure your code is well-documented and organized, and that your report is well-written and easy to follow. <br> <br>
Note: You do not need to perform text analysis or create word clouds for this exam. However, if you are interested in learning more about these techniques, you may want to explore them on your own as a side project.
<br><br>
Here is the rubric that I will use to grade your final exam:

| Item                        | Weight |
|-----------------------------|--------|
| Code accuracy               | 25%    |
| Code clarity and annotation | 25%    |
| Exploratory data analysis   | 25%    |
| Discussion of findings      | 25%    |

<br>
Good luck!



```python
import pandas as pd
import time
import random
from selenium import webdriver
```


```python
def get_job_postings():
    num_list = [1,2,3,4,5,7,8,9,10,11,13,14,15,16,17]
    job_postings=[]
    for i in num_list:
        #get job title
        job_title_element = driver.find_element('xpath','//*[@id="mosaic-provider-jobcards"]/ul/li[{}]/div/div[1]/div/div[1]/div/table[1]/tbody/tr/td/div[1]/h2'.format(i))
        job_title = job_title_element.text.replace('\n','').strip()
        
        #get company name
        company_element = driver.find_element('xpath','//*[@id="mosaic-provider-jobcards"]/ul/li[{}]/div/div[1]/div/div[1]/div/table[1]/tbody/tr/td/div[2]/span[1]'.format(i))
        company_name = company_element.text.replace('\n','').strip()

        #get job location
        job_location_element = driver.find_element('xpath','//*[@id="mosaic-provider-jobcards"]/ul/li[{}]/div/div[1]/div/div[1]/div/table[1]/tbody/tr/td/div[2]/div'.format(i))
        job_location = job_location_element.text.replace('\n','').strip().split('+')
        
        #get job description
        job_description_element = driver.find_element('xpath','//*[@id="mosaic-provider-jobcards"]/ul/li[{}]/div/div[1]/div/div[1]/div/table[2]/tbody/tr[2]/td/div/div'.format(i))
        job_description = job_description_element.text.replace('\n','')

        #get date posted
        date_posted_element = driver.find_element('xpath','//*[@id="mosaic-provider-jobcards"]/ul/li[{}]/div/div[1]/div/div[1]/div/table[2]/tbody/tr[2]/td/div/span'.format(i))
        date_posted = date_posted_element.text.replace('\n','').strip()
        
        job_postings.append([job_title, company_name, job_description, job_location[0], date_posted])
    return job_postings
```


```python
#define a function to get all useful data 
def get_data():
    num_list = [1,2,3,4,5,7,8,9,10,11,13,14,15,16,17]
    job_postings=[]
    for i in num_list:
        #get job title
        job_title_element = driver.find_element('xpath','//*[@id="mosaic-provider-jobcards"]/ul/li[{}]/div/div[1]/div/div[1]/div/table[1]/tbody/tr/td/div[1]/h2'.format(i))
        job_title = job_title_element.text.replace('\n','').strip()
        
        #get company name
        company_element = driver.find_element('xpath','//*[@id="mosaic-provider-jobcards"]/ul/li[{}]/div/div[1]/div/div[1]/div/table[1]/tbody/tr/td/div[2]/span[1]'.format(i))
        company_name = company_element.text.replace('\n','').strip()

        #get job location
        job_location_element = driver.find_element('xpath','//*[@id="mosaic-provider-jobcards"]/ul/li[{}]/div/div[1]/div/div[1]/div/table[1]/tbody/tr/td/div[2]/div'.format(i))
        job_location = job_location_element.text.replace('\n','').strip().split('+')
        
        #get job description
        job_description_element = driver.find_element('xpath','//*[@id="mosaic-provider-jobcards"]/ul/li[{}]/div/div[1]/div/div[1]/div/table[2]/tbody/tr[2]/td/div/div'.format(i))
        job_description = job_description_element.text.replace('\n','')

        #get date posted
        date_posted_element = driver.find_element('xpath','//*[@id="mosaic-provider-jobcards"]/ul/li[{}]/div/div[1]/div/div[1]/div/table[2]/tbody/tr[2]/td/div/span'.format(i))
        date_posted = date_posted_element.text.replace('\n','').strip()
        
        job_postings.append([job_title, company_name, job_description, job_location[0], date_posted])
    return job_postings
```


```python
df_out=pd.DataFrame()

#for loop to scrape multiple (100) pages of data
for j in range(100):
    driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    driver.get('https://www.indeed.com/jobs?q=Data+Analytics&l=Boston%2C+MA&from=searchOnHP&vjk=2cfb4d066248ad09')
    try:

        #scraping data from a single page 
        data=get_job_postings()

        #df with the current page data
        df_single=pd.DataFrame(data, columns=['job title', 'company name','job description', 'location','date posted'])

        #append df together
        df_out=pd.concat([df_out, df_single])

        #click on the next page button
        button = driver.find_element('xpath',"//*[@data-testid='pagination-page-next']")
        button.click()

        driver.quit() 
        
        #wait between 2-3 seconds
        time.sleep(random.uniform(2,3))
    except:
        print("Error!")
```

    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')
    /var/folders/_w/thrpsqmx1p3chfcj0gc88j8r0000gn/T/ipykernel_37693/161024681.py:5: DeprecationWarning: executable_path has been deprecated, please pass in a Service object
      driver=webdriver.Chrome('/Users/maiavachon/Downloads/chromedriver')



```python
df_out
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>job title</th>
      <th>company name</th>
      <th>job description</th>
      <th>location</th>
      <th>date posted</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Guidance and Patient Support, Commercial Analy...</td>
      <td>Vertex Pharmaceuticals</td>
      <td>Skilled in accessing data from data lakes and ...</td>
      <td>Boston, MA 02110 (Downtown Crossing area)</td>
      <td>PostedPosted 30+ days ago</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Data Analyst</td>
      <td>DumontJanks</td>
      <td>Data analytics, numbers, data visualization.Yo...</td>
      <td>Boston, MA 02111 (Chinatown area)</td>
      <td>EmployerActive 3 days ago</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Temp Research Assistant - Data Analytics</td>
      <td>Brandeis University</td>
      <td>Analysis of market data, research findings, an...</td>
      <td>Waltham, MA 02454</td>
      <td>PostedPosted 30+ days ago</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Operations Manager</td>
      <td>Forge</td>
      <td>Strong knowledge of data analysis and performa...</td>
      <td>Newton, MA</td>
      <td>PostedPosted 30+ days ago</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Pet Resort Manager</td>
      <td>Pooch Hotel</td>
      <td>Business management skills including reporting...</td>
      <td>Newton, MA 02460 (Newtonville area)</td>
      <td>PostedPosted 4 days ago</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Data Scientist</td>
      <td>Sun Life</td>
      <td>Apply data science techniques to solve busines...</td>
      <td>Remote in Wellesley Hills, MA 02481</td>
      <td>PostedPosted 12 days ago</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Data Scientist</td>
      <td>L.E.K. Consulting</td>
      <td>When relevant, support Managing Directors in d...</td>
      <td>Hybrid remote in Boston, MA</td>
      <td>PostedPosted 7 days ago</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Data Analytics Specialist</td>
      <td>Primacy</td>
      <td>Development of data pipelines to extract, cons...</td>
      <td>Boston, MA</td>
      <td>PostedPosted 30+ days ago</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Data Analyst I</td>
      <td>Massachusetts General Hospital(MGH)</td>
      <td>Moura on various data focused tasks including,...</td>
      <td>Charlestown, MA 02129</td>
      <td>PostedPosted 6 days ago</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Data Engineer, Data Analytics Division FY2023.031</td>
      <td>Office of the Inspector General</td>
      <td>Finally, the Data Division looks for opportuni...</td>
      <td>Hybrid remote in Boston, MA 02108</td>
      <td>PostedPosted 14 days ago</td>
    </tr>
  </tbody>
</table>
<p>1500 rows × 5 columns</p>
</div>




```python
#analysis #1 - sorting by company

#create new column 'count' which counts the number of job titles
df_out['count']=df_out['job title'].count()

#group by company name and count the listings by company
diff_companies=df_out.groupby('company name').count()

#sort the companies by most to least listings (show top 10)
diff_companies.sort_values('count', ascending=False)['job title'].head(10)
```




    company name
    Brandeis University                     116
    Sun Life                                100
    DumontJanks                              97
    Office of the Inspector General          92
    Primacy                                  87
    Lightcast                                70
    Medidata Solutions                       70
    Liberty Mutual                           55
    Beth Israel Deaconess Medical Center     42
    Capital One                              41
    Name: job title, dtype: int64




```python
#analysis #2 - how many remote/hybrid remote jobs?

#use only location column
location=df_out['location']

#count the number of location descriptions that have the word "remote" in it
remote = 0
for i in location:
    if 'Remote' in i or 'remote' in i or 'hybrid' in i or 'Hybrid' in i:
        remote += 1
    else:
        remote += 0

print('There are ' + str(remote) + ' job opportunities that are at least partially remote.')
print('There are ' + str(len(location)-remote) + ' job opportunities that are in person.')
```

    There are 557 job opportunities that are at least partially remote.
    There are 943 job opportunities that are in person.



```python
#analysis #3 - finding frequency of certain keywords

#list of keywords to analyze
keywords = ['advanced', 'analysis', 'visualization', 'experience', 'team']

#count the frequency of each keyword in job descriptions
keyword_counts = {}
for keyword in keywords:
    keyword_counts[keyword] = df_out['job description'].str.contains(keyword, case=False).sum()

#display the frequency of keywords
for keyword, count in keyword_counts.items():
    print(str(keyword) + ': ' + str(count))
```

    advanced: 81
    analysis: 553
    visualization: 96
    experience: 229
    team: 169


The findings of this project are as follows:

1. The number of active job listings that a particular company is promoting may affect interest on Indeed. Without further analysis, we cannot say whether this has a positive or negative relationship in hiring new employees, it does highlight a few logical points. A larger number of listings indicates a significant presence on Indeed and potentially means that the company is offering a wide range of job opportunities. Job seekers may consider exploring career options with this company and may feel more comfortable doing so with a larger company with more of these opportunities. For example, in the table below, it is clear that Brandeis University is dominating in terms of active job listings, with 116 opportunities available. Further data could suggest that Brandeis has more success in hiring new employees quicker because of the range of opportunities available as well as the exposure on the website. However, this is not to say that a job at a company such as Capital One, with 41 active listings, is any less valuable. Of course, it depends on the reputation of the company itself and the types of jobs that are in demand at a certain time. But this data does indicate that there MAY be a relationship between number of active job listings and hiring success rate.

company name
Brandeis University                     116
Sun Life                                100
DumontJanks                              97
Office of the Inspector General          92
Primacy                                  87
Lightcast                                70
Medidata Solutions                       70
Liberty Mutual                           55
Beth Israel Deaconess Medical Center     42
Capital One                              41
Name: job title, dtype: int64

2. The distribution of remote and in-person job opportunities within this dataset could be helpful in understanding the current needs of job seekers and employers. Job seekers can use this information to focus their search on Indeed based on the type of work arrangement that aligns with their goals. Employers and policymakers can also utilize this information to understand the current landscape of remote work opportunities and make informed decisions regarding workforce policies and job market trends. Especially with the pandemic, there has been a consistent rise in job opportunities that are at least partially remote for health safety and convenience. It is important to understand how the ratios between remote and in-person opportunities are changing in the coming years to better understand the goals of both job seekers and employers. For example, in the section below, there is clear information on these numbers. Out of 1500 data analytics jobs listed on Indeed for Boston MA, 557 of them are offered as hybrid or fully remote and 943 do not mention those capabilities. 

There are 557 job opportunities that are at least partially remote.
There are 943 job opportunities that are in person.

3. The prominence of certain keywords in the Indeed job descriptions may indicate employer preferences for skills and experience in positions at their companies. Job seekers can use this information to tailor their resumes and highlight relevant skills and experiences that certain employers will prioritize. Employers can also use this information to gain insights into industry trends and preferences when crafting job descriptions and assessing job seeker's profiles. For example, the searched keywords for this dataset were "advanced", "analysis", 'visualization', 'experience', and 'team'. The counts shown below suggest that employers are seeking candidates with advanced skills/knowledge in data analysis and data visualization. It also shows that prior experience in related fields could make a profile stand out and that working in teams is crucial for many of the job opportunities in the field. Using this code, any keyword could be searched and counts data will be provided. This would be incredibly useful to all participants on Indeed to better understand the data analytics job market in the area.

advanced: 81
analysis: 553
visualization: 96
experience: 229
team: 169


```python

```
