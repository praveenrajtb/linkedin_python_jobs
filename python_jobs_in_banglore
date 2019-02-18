  from time import sleep
from selenium import webdriver
from parsel import Selector
import parameters

# Create new Instance of Chrome
driver = webdriver.Chrome('/home/praveen/anaconda3/chromedriver')
sleep(5)

# Go to desired website
driver.get('https://www.linkedin.com')

# Wait 5 seconds for page to load
sleep(5)

# Locate input tag to input username, submit the username and wait 1 sec to behave like normal user
user_name =  driver.find_element_by_class_name('login-email')
user_name.send_keys(parameters.username)
sleep(1)

# Locate input tag to input password, submit the password and wait 1 sec to behave like normal user
user_pw = driver.find_element_by_id('login-password')
user_pw.send_keys(parameters.password)
sleep(1)

# Locate signin button and login to the website, click and wait 10 sec for the page to completely load 
sign_in_button = driver.find_element_by_xpath('//*[@type="submit"]')
sign_in_button.click()
sleep(10)

# Go to desired website to serach for jobs and wait 5 sec for the page to completely load 
driver.get('https://www.google.com')
sleep(5)

# Locate input tag , inpput the keywords to be serached , take the normal user time for entering the keywords
search_input = driver.find_element_by_name('q')
search_input.send_keys(parameters.search_query)
sleep(3)

# Locate search button click the button and go to the website, click and wait 5 sec for the page to completely load 

search_button = driver.find_element_by_name('btnK')
search_button.click()
sleep(5)

# empty list to store the urls
url_list = []

# create Selector instance to scrape the content
sel = Selector(driver.page_source)

# Locate tags with urls
linkedin_urls = sel.xpath('//*[@class="r"]')

# iterate ove the url tags and append each url to the list
for url in linkedin_urls:	
	url_list.append(url.xpath('.//a/@href').extract_first())

# Go to the desired webpage using the url list and wait 10 sec to completely load the page
jobs_full = []

# Iterate over the list of urls
for url in url_list:
	# Go to the desired webpage using the url list and wait 10 sec to completely load the page 
	driver.get(url)
	sleep(10)

	# create Selector instance to scrape the content
	sel = Selector(driver.page_source)

	# find jobs tags
	jobs = sel.xpath('//*[contains(@class, "job-card-search__content-wrapper")]')

	# Iterate over the job
	for job in jobs:
		# Extract Job Post, Comapny Name & Location respectively
		job_title = job.xpath('.//h3/a/text()').extract_first().strip()
		company_name = job.xpath('.//h4/a/text()').extract_first().strip()
		job_location = job.xpath('.//h5/text()')[1].extract().strip()

		# Assign to dictionary key values
		single_job = {'Job Post': job_title, 'Company': company_name, 'Job Location': job_location}
		
		# Append to the list created outside
		jobs_full.append(single_job)

		print('\n')
		print(f'job_title : {job_title}')
		print(f'company_name : {company_name}')
		print(f'job_location : {job_location}')	
		print('\n')

		
# Quit driver instance 
driver.quit()
