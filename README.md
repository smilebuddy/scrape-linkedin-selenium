# scrape_linkedin

## Introduction

`scrape_linkedin` is a python package to scrape all details from public LinkedIn
profiles, turning the data into structured json. You can scrape Companies
and user profiles with this package.

**Warning**: LinkedIn has strong anti-scraping policies, they may blacklist ips making
unauthenticated or unusual requests

## Table of Contents

<!--ts-->

- [scrape_linkedin](#scrapelinkedin)
    - [Introduction](#introduction)
    - [Table of Contents](#table-of-contents)
    - [Installation](#installation)
        - [Install with pip](#install-with-pip)
        - [Install from source](#install-from-source)
        - [Tests](#tests)
    - [Getting & Setting LI_AT](#getting--setting-liat)
        - [Getting LI_AT](#getting-liat)
        - [Setting LI_AT](#setting-liat)
    - [Examples](#examples)
    - [Usage](#usage)
        - [Command Line](#command-line)
        - [Python Package](#python-package)
            - [Profiles](#profiles)
            - [Companies](#companies)
            - [config](#config)
    - [Scraping in Parallel](#scraping-in-parallel)
        - [Example](#example)
        - [Configuration](#configuration)
    - [Issues](#issues)

<!-- Added by: austinoboyle, at: 2018-05-06T20:13-04:00 -->

<!--te-->

## Installation

### Install with pip

Run `pip install git+git://github.com/austinoboyle/scrape-linkedin-selenium.git`

### Install from source

`git clone https://github.com/austinoboyle/scrape-linkedin-selenium.git`

Run `python setup.py install`

## Getting & Setting LI_AT

Because of Linkedin's anti-scraping measures, you must make your selenium
browser look like an actual user. To do this, you need to add the li_at cookie
to the selenium session.

### Getting LI_AT

1.  Navigate to www.linkedin.com and log in
2.  Open browser developer tools (Ctrl-Shift-I or right click -> inspect
    element)
3.  Select the appropriate tab for your browser (**Application** on Chrome,
    **Storage** on Firefox)
4.  Click the **Cookies** dropdown on the left-hand menu, and select the
    `www.linkedin.com` option
5.  Find and copy the li_at **value**

### Setting LI_AT
There are two ways to set your li_at cookie:

1.  Set the LI_AT environment variable
    -   `$ export LI_AT=YOUR_LI_AT_VALUE`
    -   **On Windows**: `$ set LI_AT=YOUR_LI_AT_VALUE
2.  Pass the cookie as a parameter to the Scraper object.
    > `>>> with ProfileScraper(cookie='YOUR_LI_AT_VALUE') as scraper:`

A cookie value passed directly to the Scraper **will override your
environment variable** if both are set.

## Usage
### Python Package

#### Profiles

Use `ProfileScraper` component to scrape profiles.

```python
from scrape_linkedin import ProfileScraper

with ProfileScraper() as scraper:
    profile = scraper.scrape(user='austinoboyle')
print(profile.to_dict())
```

`Profile` - the class that has properties to access all information pulled from
a profile. Also has a to_dict() method that returns all of the data as a dict

    with open('profile.html', 'r') as profile_file:
        profile = Profile(profile_file.read())

    print (profile.skills)
    # [{...} ,{...}, ...]
    print (profile.experiences)
    # {jobs: [...], volunteering: [...],...}
    print (profile.to_dict())
    # {personal_info: {...}, experiences: {...}, ...}

**Structure of the fields scraped**

-   personal_info
    -   name
    -   company
    -   school
    -   headline
    -   followers
    -   summary
-   skills
-   experiences
    -   volunteering
    -   jobs
    -   education
-   interests
-   accomplishments
    -   publications
    -   cerfifications
    -   patents
    -   courses
    -   projects
    -   honors
    -   test scores
    -   languages
    -   organizations
