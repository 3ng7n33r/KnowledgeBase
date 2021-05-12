# Django Testing

There are two types of testing that should be done within a Django project: Functional and Unit testing.
## Functional Tests
Functional testing looks at the program from the outside like a user would do. Therefore, it is a good idea to tell a user story and narrate your way through the functional tests. Here is an example from the TDD for Python book (functional_tests.py):
```py
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import time
import unittest

class NewVisitorTest(unittest.TestCase) :

	def setUp(self):
		self.browser = webdriver.Firefox()
		
	def tearDown(self):
		self.browser.quit()
		
	def test_can_start_a_list_and_retrieve_it_later(self):
		# Edith has heard about a cool new online to-do app. She goes
		# to check out its homepage
		self.browser.get('http://localhost:8000')

		# She notices the page title and header mention to-do lists
		self.assertIn('To-Do', self.browser.title)
		header_text = self.browser.find_element_by_tag_name('h1').text
		self.assertIn('To-Do', header_text)

		# She is invited to enter a to-do item straight away
		inputbox = self.browser.find_element_by_id('id_new_item')
		self.assertEqual(
			inputbox.get_attribute('placeholder'),
			'Enter a to-do item'
		)
		
		# She types "Buy peacock feathers" into a text box (Edith's hobby
		# is tying fly-fishing lures)
		inputbox.send_keys('Buy peacock feathers')

		# When she hits enter, the page updates, and now the page lists
		# "1: Buy peacock feathers" as an item in a to-do list
		inputbox.send_keys(Keys.ENTER)
		time.sleep(1)
		
		table = self.browser.find_element_by_id('id_list_table')
		rows = table.find_elements_by_tag_name('tr')
		self.assertTrue(
			any(row.text == '1: Buy peacock feathers' for row in rows),
			"New to-do item did not appear in table"
		)
```

## Unit tests
Unit tests look at the program from the inside, like a programmer does. They are more incremental and don't follow a narrative path. Instead, they test all lines of code that have to be written to make the functional test pass. Multiple unit tests can be necessary to pass a single functional test. Unit tests would be found within the app directory, either directly in the tests.py file or a little bit cleaner in a tests folder, split into test_model, test_view etc. (Don't forget to add an empty \_\_init__.py file into the folder). A unit tests could look like this (project/app/tests.py):
```py
from django.test import TestCase
from django.urls import resolve
from django.http import HttpRequest
from django.template.loader import render_to_string

from lists.views import home_page

class HomePageTest(TestCase):

	def test_root_url_resolves_to_home_page_view(self):
		found = resolve('/')
		self.assertEqual(found.func, home_page)
		
	def test_home_page_returns_correct_html(self):
		response = self.client.get('/')
		self.assertTemplateUsed(response, 'home.html')
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTcwOTAxNjg5MF19
-->