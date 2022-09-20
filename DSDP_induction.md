---
theme: gaia
size: 16:9
_class: lead
paginate: true
backgroundColor: #fff
#backgroundImage: url('https://marp.app/assets/hero-background.svg')
marp: true
style: |
  p, li, {
    font-size: 32px;
  }
---

# **A practical guide to data science projects**

Peter Baudains

[CDRC](https://cdrc.ac.uk) Research Data Scientist

---
## Contents

1. Introduction 
2. Reproducibility: Data, Code, and Environments
3. (Handling threats to) Openness: Data Security and Maintaining IP 
4. Delivering impact: Developing and publishing data products
5. [CDRC Data Store](https://data.cdrc.ac.uk) 
6. [CDRC Apps](https://apps.cdrc.ac.uk)

---
## 1. Introduction

Publicly-funded data science research should be...

1. Reproducible
2. Open
3. Impactful

---
### 1.1 Reproducible
Anyone with access to your data and code (which should be as many people as possible) should be able to generate the same conclusions as you did. This strengthens confidence in your findings and is an essential part of the scientific process.

---
### 1.2 Open/Transparent
The legal and ethical requirements for data security notwithstanding, every effort should be made to ensure openness and transparency in how a particular result or finding has been produced. 

Two challenges to openness are ensuring any data security requirements and maintaining intellectual property of the resulting work.

---
### 1.3 Impactful
The tagline to the Data Science Development Programme is:

*"Data Science for Public Good"*

Data Science is increasingly being used to provide insights and make decisions in different domains. Every project on this year's programme has the ability have a meaningful impact. 

It's well worth identifying the kinds of impact that might be made early on in the project.

---

### 1.4 Goal of the session

The goal of this session is to introduce some surrounding tools and technology that will be helpful in your projects for achieving the three goals we have just covered. It's also to get you thinking about **how** you tackle your projects from a practical perspective.

The majority of the examples in this session are based from my experience using Python, however, I think most of the content is transferable to any language. 

Also, apologies to those for which the content in this session may seem obvious. Hopefully there will still be a few nuggets for you to take away. 

---

### 1.5 A note on CLIs (Command Line Interfaces)

Some of the code snippets used in this presentation are intended to be run on a command line interface to the machine you are using (e.g. cmd in Windows, Terminal on Linux/Mac). 

Being comfortable using some command line tools is a really valuable skill. There's plenty of free online resources available for learning the basics:

1. https://ubuntu.com/tutorials/command-line-for-beginners
2. https://www.freecodecamp.org/news/command-line-for-beginners/
3. [Windows Command Line Tutorials by TheNewBoston](https://www.youtube.com/playlist?list=PL6gx4Cwl9DGDV6SnbINlVUd0o2xT4JbMu)


---


## 2. Reproducibility: Data, Code and Environments

 I'll start with an example...

```
jupyter lab
```

<!--- This example will demonstrate some common pitfalls in doing data science, hitting on some of the issues that may arise from the below sections--->

---

### Q. How to approach this with reproducibility in mind?

Some practical tips: 

2.1. Separate data from code
2.2. Separate raw data from processed data
2.3. Retain code for converting raw data to processed data
2.4. Build and think in terms of pipelines
2.5. Modularise and test your code
2.6. Version control your code
2.7. Document your code dependencies
2.8. Tools for replicating research environments

---

### 2.1 Separate data from code

Set up two directories within your working environment: 

```
~/Data/
~/Code/
```
Then, within your code, create a variable to point to your data directory:

```python
import pandas as pd

data_directory = '~/Data/'

df = pd.read_csv(data_directory + 'data_file.csv')
```

---

### 2.2 Separate raw data from processed data (1)

Actually, it would be better to have something like: 
```
~/Data/Raw/
~/Data/Processed/
```
Where the raw data directory contains your raw data at the start of the project and is only ever read from in your code.

The processed directory is where data can be written. This might contain final datasets for publication. It might also contain intermediate results so that you don't need to re-run the pipeline each time. 

---

### 2.2 Separate raw data from processed data (2)

If you're using a database management system or other type of data storage then this pattern should be replicated. For example if using a SQL database: 

```sql
CREATE TABLE rawData (...)
CREATE TABLE derivedDataSet1 (...)
```
Then only run ```SELECT``` queries on ```rawData``` leaving data updates and data transformations for processed data tables.

---

### 2.3 Retain ALL code for converting raw to processed data

Remember that reproducibility means that if we start from the raw data using your code, it is possible to follow it step-by-step and end up with your results. You can only do this if you retain every piece of code used to generate a dataset in your Processed data file.

---

### 2.4 Build and think in terms of pipelines

Your project code can usually be thought in terms of a pipeline or set of pipelines that take in some raw data and generate some processed data. 

![](images/pipeline-model.png)

<span style="font-size:50%">https://www.databricks.com/glossary/what-are-ml-pipelines</span>

---

### 2.5 Modularise and test your code

Thinking of your project in terms of pipelines can help identify how to _modularise_ your code. 

Make use of functions/package capability in your code and try to design your code so that each component does a single thing. 

Each module can then be tested independently, making debugging easier and minimising the risk of error propagation.

--- 

### 2.6 Version control your code (1)

[Git](https://git-scm.com/) is the most widely used version control system. It can be used on its own without a connection to the internet for monitoring and tracking changes to code. A version control system should always be used to keep a record of changes and to allow the return to a previous version. 

```git 
git init 
git add .
git commit -m "Really helpful commit message here"
```

```git
git status
git log
```

---

### 2.6 Version control your code (2)

[GitHub](https://github.com) is a hosting service for your code. It syncs seamlessly with Git and acts as a remote backup for your code. Repositories on GitHub can be made private or public. 

A public GitHub repository is a great option for making your research openly available.

A really handy guide for using Git (and GitHub as a remote server) is here: https://rogerdudler.github.io/git-guide/

---

### 2.7 Version control your data

Some projects will start with initial invariant datasets and proceed in a data pipeline until some results are generated. In these cases, if a permanent link to a dataset in a data repository can be made and the code is publicly available, then the results can be reproduced. 

Other projects will have datasets that change over time. 

Git can be used to version control small datasets, but isn't really designed for large files.

In many cases, reproducibility can be accomplished by having a research data management plan.

---

### 2.8 Research data management plans

A research data management plan details each stage of the research data lifecycle, including data collection, storage and analysis, documentation and metadata, and long-term preservation of the data for reproducibility. 

It should also identify the roles and responsibilities for those involved in the research. 

This may involve identifying individuals who may need to provide support for research environments. 

---

### 2.9 Document your code dependencies (1)

When you install a software package using a package installer/manager such as pip, conda or R/RStudio, it installs a version of the package and a version of each of that package's dependencies.

If we're aiming for reproducibility, we need to document which versions of which packages have been used to generate a particular result. If we always use the latest version of the code, then the functionality may change, which could alter the result.

---

### 2.9 Document your code dependencies (2)

Package managers typically have capability to generate a list of dependencies within any existing environment e.g.:

```conda
conda list -n environment_name
```

will list all dependencies and versions of those dependencies. This can be used to automatically create the same environment (i.e. with the same versions of packages) which were used to generate your results. 


---

### 2.10 Tools for replicating research environments

Conda 
Binder
Docker

---

### 2.11 Exercise: Create a template data science project directory




---

## 3. (Handling threats to) Openness: Data security and maintaining IP

Data relating to individuals is likely to be subject to data protection legislation which forces anyone that controls or processes the data to take necessary steps to protect it.

Data may also require protection due to commercial sensitivity or other licensing arrangement.

Theft of intellectual property is another barrier to openness. 

This section discusses how to tackle some of these.

---
### 3.1 Data Licensing and Access Tiers

The Consumer Data Research Centre here at Leeds and other organisations such as the UK Data Service use a three-levelled approach for defining data access tiers relating to the potential sensitivity data: 

1. Open Data
2. Safeguarded Data
3. Controlled/Secure Data

We discuss each of these in turn below.

---

#### 3.1.1 Open Data

Open data is free to download and use and will contain no personal or disclosive information. 

Open data is usually provided with an open data license such as an Open Government License or other open software license such as MIT License. 

Even though the data is open, you will need to be mindful of any particular conditions of the use which will be described in the license. This might include attribution conditions, for example.

---

#### 3.1.2 Safeguarded Data

Safeguarded data will not contain any personally identifiable information but may have some special conditions attached to its use, such as a user license that prevents certain uses and publication of the data. 

Safeguarded data may arise from commercial sensitivity, e.g. when a commercial organisation wishes to limit the accessibility of a dataset as it may contain data that gives competitors an advantage.

Safeguarded datasets are available via the CDRC and other data services subject to end user agreements and the completion of other conditions such as obtaining UK Data Service Safe Researcher Status.

---

#### 3.1.3 Controlled/Secure Data

This is the highest tier within the CDRC Data Service and may contain data that risks disclosing individuals or which is considered to be highly commercially sensitive. 

At the University of Leeds, controlled/secure datasets can only be access via the Leeds Analytic Secure Environment for Research (LASER), a secure research environment. 

The data license may also stipulate that access to the data can only be granted via a physical safe room, providing further assurance against the risk of any data breach.


---

### 3.2 Meeting FAIR principles for non-open data




---

### 3.3 Creating a DOI for a GitHub repository




---

# 4. Delivering impact: Developing and publishing data products


---

### 4.1 What is a data product?



---

### 4.2 Product lifecycle 

Also known as the software development lifecycle.

---

#### 4.2.1 Analysis & Design

Audience, data, algorithms, design

---

#### 4.2.2 Development & Testing

Web frameworks such as RShiny/Plotly-Dash, etc.


---

#### 4.2.3 Deployment and Maintenance

Dockerized applications

Github actions for automated deployment

The CDRC has a cloud-based web-app hosting capability which can be made available for you to host your data products.

Free hosting is also available from a variety of services.

---

### 4.3 The Agile Manifesto
 
The SDLC as I've just presented appears extremely linear (also known as *waterfall*). In real life development and data science projects, it's extremely rare to follow such a linear process. 

*Requirements typically change as the project evolves.*

The Agile Manifesto was written by a group of software engineers in the early 2000s and has had a massive impact on how software engineering is done in industry.

---

### 4.4 



---


## 5. CDRC Data Store

The CDRC data store is a front-end to our data service. A number of open datasets are available for download following a simple user registration: 

https://data.cdrc.ac.uk


---

## 6. CDRC Apps

CDRC has resources for hosting web apps such as dashboards or other visualisation tools. 

Our current apps across Leeds, UCL, Liverpool and Oxford partners can be seen here:
https://apps.cdrc.ac.uk

If you have an idea for a web-app that comes from your research then please get in touch at: p.baudains@leeds.ac.uk


---

## 7. References and further reading

git - the simple guide: https://rogerdudler.github.io/git-guide/

https://docs.github.com/en/repositories/archiving-a-github-repository/referencing-and-citing-content

https://agilemanifesto.org/

https://the-turing-way.netlify.app/welcome.html

Vincent D. Warmerdam at PyData Eindhoven 2019 on Python/Pandas workflows: https://www.youtube.com/watch?v=yXGCKqo5cEY

Wilkinson, M., Dumontier, M., Aalbersberg, I. et al. The FAIR Guiding Principles for scientific data management and stewardship. Sci Data 3, 160018 (2016). https://doi.org/10.1038/sdata.2016.18
