Using URL Composition to Predict Phishing Attempts
===

![](https://github.com/ray-zapata/project_classification_phishing/blob/main/assets/logo.jpg)

### Table of Contents
---

I.   [Project Overview             ](#i-project-overview)
1.   [Description                  ](#1-description)
2.   [Deliverables                 ](#2-deliverables)
2.   [Web App                      ](#3-web-app)

II.  [Project Summary              ](#ii-project-summary)
1.   [Goals                        ](#1-goals)
2.   [Initial Thoughts & Hypothesis](#2-initial-thoughts--hypothesis)
3.   [Findings & Next Phase        ](#3-findings--next-phase)

III. [Data Context                 ](#iii-data-context)
1.   [Data Dictionary              ](#data-dictionary)

IV.  [Process                      ](#iv-process)
1.   [Project Planning             ](#1-project-planning)
2.   [Data Acquisition             ](#2-data-acquisition)
3.   [Data Preparation             ](#3-data-preparation)
4.   [Data Exploration             ](#4-data-exploration)
5.   [Modeling & Evaluation        ](#5-modeling--evaluation)
6.   [Product Delivery             ](#6-product-delivery)

V.   [Modules                      ](#v-modules)

VI.  [Project Reproduction         ](#vi-project-reproduction)

<br>

![](https://github.com/ray-zapata/project_classification_phishing/blob/main/assets/divider.png)

<br>

### I. Project Overview
---

#### 1. Description

One of the biggest internal problems with any company relying on electronic communications is employees falling victim to phishing schemes to obtain private, personal, and sensitive information. These attempts make every individual in the workforce a potential vector for information leaks that would result in server failures, customer data compromise, and operation security breaches.

#### 2. Deliverables

- Jupyter [notebook](https://nbviewer.jupyter.org/github/ray-zapata/project_classification_phishing/blob/main/phishing_report.ipynb) containing findings, summary, and process through data science pipeline
- Trello [board](https://trello.com/b/Zl97PmXz/phishing-classification) demonstrating process and planning
- This [README](#using-url-composition-to-predict-phishing-attempts) containing project summary, goals, and findings

#### 3. Web App

In addition to the above deliverables, a web app was created to demonstrate how effective the classifier could be in the field. This is a simple proof of concept, and with additional time may be expanded on to use domains known to be used in phishing schemes. This does not serve as a layer of protection, and should in no way be used as security measure.

- [Streamlit App](https://share.streamlit.io/ray-zapata/project_classification_phishing/main/webapp.py)

### II. Project Summary
---

#### 1. Goals

The goal of this project is to create a classification model that is capable of maximizing the capture of phishing attempts utilizing only the composition of a Universal Resource Locator (URL), also known colloquially as a web address, while minimizing instances of false positives that may result in productivity loss.

#### 2. Initial Thoughts & Hypothesis

In the initial phases of this project, the working hypotheses were that the URL length in would be a strong feature in predicting phishing attempts. The count of special characters, such as dots `.` and hyphens `-`, was also considered to be a strong driver in this endeavor due to anecdotal evidence suggesting phishing attempts more frequently make use of subdomains and high path levels to mask the underlying hostname as a legitimate source (i.e. `http://https.apple.com.nz/apple-id/login/-password/%amp;2993/ref?q=dds99kdjf`)

#### 3. Findings & Next Phase

There would prove to be some legitimacy to the above hypotheses, as modeling would utilize recommended features that included these special characters as well as the path level. It was also found that by using clustering methodology to create groupings based on the number of dots to the number of dashes showed statistically significant sample means different from the population mean using analysis of variance (ANOVA) testing.

In modeling, the most successfully train fitted model was a random forest model that produced results which capture over 90%
of positive class for the target `is_phishing_attempt`. This model maintained an above 86% overall accuracy throughout creation, evaluation, and testing. The strongest drivers for determining if a URL was a phishing attempt were: `num_dash_url`, `num_dot_url`, `num_numerics`, `num_queries`, `num_sensitive_words`, `path_length`, `path_level`, `query_length`, `url_char_length`, and `dash_dot_clstr_2`. These features were selected through recursive feature elimination using the RandomForestClassifier as the model.

With additional time, it would be worthwhile to use analysis of the web page HTML to find any frequent elements common among illegitimate links. For the purpose of internal security, in actual application a machine dedicated to rendering the pages in question would be the best course of action to prevent shadow installation of common malware or other malicious software.

### III. Data Context
---

The data utilized throughout this project was acquired from [Kaggle](https://www.kaggle.com/shashwatwork/phishing-dataset-for-machine-learning). This data was collected by [*Choon Lin Tan*](https://data.mendeley.com/datasets/h3cgnj8hft/1) and prepared for machine learning by Kaggle user *Shashwat Tiwari*. The data has been prepared in a manner that makes significant preparation unnecessary; however, much of the initial data will be unused to achieve the goals of this project.

#### Data Dictionary

Following acquisition of CSV from Kaggle, linked above, the DataFrames used in this project contain the following variables. Contained values are defined along with their respective data types.

| Variable              | Definition                                         | Data Type |
|:---------------------:|:--------------------------------------------------:|:---------:|
| dash_dot_clstr_#      | cluster created for count of dash to dots in URL   |   uint8   |
| domain_in_path        | boolean if domain is in path                       |   int64   |
| domain_in_subdomain   | boolean if domain is also in subdomain             |   int64   |
| has_random_string     | boolean if URL has a string of random values       |   int64   |
| has_tilde             | boolean if URL has tilde (`~`)                     |   int64   |
| hostname_length       | numeric length of hostname in URL                  |   int64   |
| is_phishing_attempt * | boolean if URL is phishing attempt                 |   int64   |
| num_ampersand         | count of ampersands in URL                         |   int64   |
| num_dash_hostname     | count of dashes (`-`) in URL hostname              |   int64   |
| num_dash_url          | count of dashes in entire URL                      |   int64   |
| num_dot_url           | count of dots (`.`) in URL                         |   int64   |
| num_numerics          | count of numeric values in URL                     |   int64   |
| num_percent_sign      | count of percent signs (`%`) in URL                |   int64   |
| num_queries           | count of queries in URL                            |   int64   |
| num_sensitive_words   | count of sensitive words (i.e. "password") in URL  |   int64   |
| num_underscore_url    | count of underscores (`_`) in URL                  |   int64   |
| path_length           | numeric length of path                             |   int64   |
| path_level            | count of path levels in URL                        |   int64   |
| query_length          | length of all queries in URL                       |   int64   |
| subdomain_level       | count of subdomain levels in URL                   |   int64   |
| url_char_length       | numeric length of overall URL                      |   int64   |

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  * Target variable

### IV. Process
---

#### 1. Project Planning
🟢 **Plan** ➜ ☐ _Acquire_ ➜ ☐ _Prepare_ ➜ ☐ _Explore_ ➜ ☐ _Model_ ➜ ☐ _Deliver_

- [x] Build this README containing:
    - Project overview
    - Initial thoughts and hypotheses
    - Project summary
    - Instructions to reproduce
- [x] Plan stages of project and consider needs versus desires

#### 2. Data Acquisition
✓ _Plan_ ➜ 🟢 **Acquire** ➜ ☐ _Prepare_ ➜ ☐ _Explore_ ➜ ☐ _Model_ ➜ ☐ _Deliver_

- [x] Obtain initial data and understand its structure
- [x] Remedy any inconsistencies, duplicates, or structural problems within data
- [x] Perform data summation

#### 3. Data Preparation
✓ _Plan_ ➜ ✓ _Acquire_ ➜ 🟢 **Prepare** ➜ ☐ _Explore_ ➜ ☐ _Model_ ➜ ☐ _Deliver_

- [x] Address missing or inappropriate values, including outliers
- [x] Plot distributions of variables
- [x] Encode categorical variables
- [x] Consider and create new features as needed
- [x] Split data into `train`, `validate`, and `test`

#### 4. Data Exploration
✓ _Plan_ ➜ ✓ _Acquire_ ➜ ✓ _Prepare_ ➜ 🟢 **Explore** ➜ ☐ _Model_ ➜ ☐ _Deliver_

- [x] Visualize relationships of variables
- [x] Formulate hypotheses
- [x] Perform statistical tests
- [x] Decide upon features and models to be used

#### 5. Modeling & Evaluation
✓ _Plan_ ➜ ✓ _Acquire_ ➜ ✓ _Prepare_ ➜ ✓ _Explore_ ➜ 🟢 **Model** ➜ ☐ _Deliver_

- [x] Establish baseline prediction
- [x] Create, fit, and predict with models
- [x] Evaluate models with out-of-sample data
- [x] Utilize best performing model on `test` data
- [x] Summarize, visualize, and interpret findings

#### 6. Product Delivery
✓ _Plan_ ➜ ✓ _Acquire_ ➜ ✓ _Prepare_ ➜ ✓ _Explore_ ➜ ✓ _Model_ ➜ 🟢 **Deliver**
- [x] Prepare Jupyter Notebook of project details through data science pipeline
    - Python code clearly commented when necessary
    - Sufficiently utilize markdown
    - Appropriately title notebook and sections
- [x] With additional time, continue with exploration beyond MVP
- [x] With more time, create web app to put classifier into practice
- [x] Proof read and complete README and project repository

### V. Modules
---

The created modules used in this project below contain full comments an docstrings to better understand their operation. Where applicable, all functions used `random_state=19` at all times.

- [`acquire.py`](https://raw.githubusercontent.com/ray-zapata/project_classification_phishing/main/acquire.py): contains functions used in initial data acquisition leading into the prepare phase
- [`prepare.py`](https://raw.githubusercontent.com/ray-zapata/project_classification_phishing/main/prepare.py): contains functions used to prepare data for exploration and visualization
- [`explore.py`](https://raw.githubusercontent.com/ray-zapata/project_classification_phishing/main/explore.py): contains functions to visualize the prepared data and estimate the best drivers of property value
- [`model.py`  ](https://raw.githubusercontent.com/ray-zapata/project_classification_phishing/main/model.py): contains functions to create, test models and visualize their performance

### VI. Project Reproduction
---

To being reproducing the results in this project, you must first full read this README and obtain the CSV file linked in Section III. The steps documented in the above process are key to project reproduction. There is minimal preparation needed to recreate the above findings, using the wrangle function to drop columns and rename retained once you've obtained the data file. When using the above modules, ensure full reading of docstrings and comments to understand function purpose and scope. 

[[Return to Top]](#using-url-composition-to-predict-phishing-attempts)
