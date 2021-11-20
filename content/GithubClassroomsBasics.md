<!--
author:   Sebastian Zug, André Dietrich
email:    sebastian.zug@informatik.tu-freiberg.de, andre.dietrich@informatik.tu-freiberg.de,
version:  0.0.1

language: en
narrator: US English Male

import: https://github.com/LiaTemplates/Pyodide

comment:  Describes the application of github2pandas_manager for Github-Classroom projects
translation: Deutsch  translations/German.md
translation: Français translations/French.md
-->

[![LiaScript](https://raw.githubusercontent.com/LiaScript/LiaScript/master/badges/course.svg)](https://liascript.github.io/course/?https://raw.githubusercontent.com/TUBAF-IFI-DiPiT/github2pandas_tutorials/main/content/GithubClassroomsBasics.md)

# Automated supervision of student activities in Github Classrooms

      --{{0}}--
Hi everyone, my name is Sebastian Zug; today I would like to introduce you to some new Python modules for analyzing student activity in a Github Classroom. The design and implementation of these packages are part of the "DiP-iT" project, a national research project at German universities. It integrates partners from Humboldt Universität zu Berlin, Otto-von-Guericke University and my affilation, TU Bergakademie Freiberg.

      {{0-1}}
![github2pandas_manager](../pics/pypi.png)

      --{{1}}--
But let's get started. Github Classrooms are an excellent way to teach students about the features and use of project management tools and version control. You define a task in a repository, and a clone is created for individual students or groups after their login. Afterward they work independently on the given task and commit a result. This can be automatically evaluated by pattern matching or test methods.

      {{1-2}}
`````````
 Teachers repository                              Student repositories
 --------------------                             --------------------

                                                         .-----------.              
                                                  ╔══════| Student 1 |══════╗           
                                        enrole    ║      '-----------'      ║
                                      +---------- ║ Digital Systems 2021    ║
                                      | +-------> ║                         ║
+------------------+                  v | clone   ║ import numpy as np      ║
| # Digital Systems|\          .-,(   ),-.        ║ ...                     ║
| (Sprint 2021)    +-+      .-(  Github   )-.     ╚═════════════════════════╝
|                    | --> (    Classroom    )                                      
|"##"Task 1          |      '-( Assignemt )-'            .-----------.         
| + Implement ...    |         '-.(   ).-'        ╔══════| Student 2 |══════╗
+--------------------+                ^ |         ║      '-----------'      ║
                                      | | clone   ║ Digital Systems 2021    ║
                                      | +-------> ║                         ║
                                      +---------- ║"#"include<stdio.h>      ║
                                                  ║ ...                     ║
                                                  ╚═════════════════════════╝

                                                            .....

`````````

      --{{2}}--
However, depending on the size of your course, supervising student's activity becomes complex. Github Classooms provides some mechanisms, but these can only answer specific questions to a limited extent. As a result, you click from student repository to repository and evaluate the results manually.

        {{2-3}}
`````````
 Teachers repository                              Student repositories
 --------------------                             --------------------

                                                         .-----------.              
                                                  ╔══════| Student 1 |══════╗           
                                        enrole    ║      '-----------'      ║
                                      +---------- ║ Digital Systems 2021    ║
                                      | +-------> ║                         ║
+------------------+                  v | clone   ║ import numpy as np      ║
| # Digital Systems|\          .-,(   ),-.        ║ ...                     ║
| (Sprint 2021)    +-+      .-(  Github   )-.     ╚═════════════════════════╝
|                    | --> (    Classroom    )                                      
|"##"Task 1          |      '-( Assignemt )-'            .-----------.         
| + Implement ...    |         '-.(   ).-'        ╔══════| Student 2 |══════╗
+--------------------+                ^ |         ║      '-----------'      ║
                                      | | clone   ║ Digital Systems 2021    ║
                                      | +-------> ║                         ║
                                      +---------- ║"#"include<stdio.h>      ║
                                                  ║ ...                     ║
                                                  ╚═════════════════════════╝

                                                             .....
                                                 |                           |
+-------------------------------+                .-------------+-------------.
| Used features:                |                              |
|    | issues | actions | ... | | <----------------------------+
| A  |    X   |    x    |     | |        manual process
| B  |    x   |         |     | |
+-------------------------------+
Report on activities
`````````

--{{3}}--
Github2pandas closes this gap and allows the automatic aggregation of repository data like commits, issues or pull requests etc. The collected information is stored either in Python pandas dataframes or in csv files. The collected data is anonymized by github2pandas and can be evaluated as feedback for the instructor.

{{3-4}}
`````````
 Teachers repository                              Student repositories
 --------------------                             --------------------

                                                         .-----------.              
                                                  ╔══════| Student 1 |══════╗           
                                        enrole    ║      '-----------'      ║
                                      +---------- ║ Digital Systems 2021    ║
                                      | +-------> ║                         ║
+------------------+                  v | clone   ║ import numpy as np      ║
| # Digital Systems|\          .-,(   ),-.        ║ ...                     ║
| (Sprint 2021)    +-+      .-(  Github   )-.     ╚═════════════════════════╝
|                    | --> (    Classroom    )                                      
|"##"Task 1          |      '-( Assignemt )-'            .-----------.         
| + Implement ...    |         '-.(   ).-'        ╔══════| Student 2 |══════╗
+--------------------+                ^ |         ║      '-----------'      ║
                                      | | clone   ║ Digital Systems 2021    ║
+--------------------+                | +-------> ║                         ║
| # Digital Systems  |\               +---------- ║"#"include<stdio.h>      ║
| Report generator   +-+                          ║ ...                     ║
|                      | <----------------+       ╚═════════════════════════╝
| import github2pandas | ---+             |            
+----------------------+    |             |                     .....
                            v             |      |                           |
+-------------------------------+         |      .-------------+-------------.
| Used features:                |         |                    |
|    | issues | actions | ... | |         +--------------------+
| A  |    X   |    x    |     | |           script based aggreation
| B  |    x   |         |     | |           & analysis
+-------------------------------+
Report on activities
`````````

## Concepts of the `github2pandas` modules

      --{{0}}--
The sequence diagram shows the aggregation phase on an abstract level. It uses the two Python modules prepared by the team. github2pandas implements the actual data aggregation from one repository, while github2pandas_manager coordinates the survey across multiple repos. The user specifies his requirements in a yaml file and passes when calling the program. Lists of repositories can be specified, but also patterns and search queries. In addition, the user defines what information - commits, issues, workflows, etc. - should be collected. Finally, Github2pandas_manager identifies the relevant repositories and collects the desired data set for each of them.  In the end, the information is saved as a pandas data frame or csv file.

       {{0-1}}
![Workflow](http://www.plantuml.com/plantuml/svg/fLDDYnD14BtthoZmeg1py9P0P4LPzR0NGJmkbUbAfxG_nghg3OlutqqFSKRSsMYmOPXYfjvxLU_HLseeLbDqs5iH-AGaRa0nxdd0R13OzdNxybXxrDk46SEv3kVHS8jAfy_mPBMwlbwjdDjiu2CDHTcAtCFh48G26fSCcurpJHTUl5gMMuFCo07DIBB2q-uUKtpc5Y1dkG9b4ZG2eM-LrFv6i0OJp9hO_a2SqS2i1vAf_pz6dFQEh3Qw-1OD7_WNInd6xjk-r6nWd4WT7C_uv_kRaXARFeSFgfMExyz5lkvYEHpBhkj-E3YTN9eiXxr1sJqstqt9RIZE0OGIScxLExRtTVjhPuZS1Bk9-DyyM4zupfxls5UCuD5mcMV6pq31mnBYWTGPnkMjrOhGI0sSOP3oXNg3NOcUP2IZx5rxBesx3XwD07_BTC_QKfy3lo49rAA-c3sDoDdEoI3OSIHzdA_ToSbMyjFcg73ogWZqUdVYkQBiQue_0G00)

      --{{1}}--
I took the following exemplary configuration file from the `github2pandas_manager` documentation. Starting in line 1 it references the project name and the data root folder. In this project the aggregation selects relevant repositories according to white and black patterns applied on repo names. Multiple entries can be inclduded in both lists, whereby black-list entries are excluded from aggregation. In the example, the issues and a complete overview of the versions of all documents is in focus of the project.

       {{1-3}}
```yaml  config_data_aggregation.yml
project_name: Demo_Project

#################################################################
# Folder structure

project_folder: ./examples/

#################################################################
# Repository selection by pattern name

repo_white_pattern:
  - "github2pandas"
  - "github2pandas_manager"  

repo_black_pattern:
  - "github2pandas_APP_swe_tu_freiberg"
  - "github2pandas_company_evaluation"

#################################################################
# Content definition
content:
  - Repository
  - Issues
```

      --{{2}}--
Running github2pandas_manager with this configuration creates two datasets that we can analyze downstream. We are going to talk about the installation process in several minutes. As you can see, we run two aggregation loops for Repository data as well as Issue data.

       {{2-4}}
```
> pipenv run python3 -m github2pandas_manager -path config_data_aggregation.yml                            ✔  Loading .env environment variables...
4 machting repositories found.
Repository -   0 /   4 - TUBAF-IFI-DiPiT/github2pandas (4822)
Repository -   1 /   4 - TUBAF-IFI-DiPiT/github2pandas_manager (4805)
Repository -   2 /   4 - TUBAF-IFI-DiPiT/github2pandas_notebooks (4788)
Repository -   3 /   4 - TUBAF-IFI-DiPiT/github2pandas_tutorials (4771)
Issues     -   0 /   4 - TUBAF-IFI-DiPiT/github2pandas (4754)
Issues     -   1 /   4 - TUBAF-IFI-DiPiT/github2pandas_manager (4751)
Issues     -   2 /   4 - TUBAF-IFI-DiPiT/github2pandas_notebooks (4750)
Issues     -   3 /   4 - TUBAF-IFI-DiPiT/github2pandas_tutorials (4749)
examples/Demo_Project/Repositories.csv
examples/Demo_Project/Issues.csv
Aus Maus
```

      --{{3}}--
For our example, I uploaded the two files to the Data folder so we can take a closer look.

       {{3-4}}
| repo_name               | creation_date       | size | contributor_count | branch_count | commit_count | commit_comment_count | last_commit_date    | labels_count | tag_count | milestone_count | pullrequest_count | pullrequest_review_count | release_count | workflow_count | readme_length | issues_count | issues_comment_count | has_wiki | has_pages | has_projects | has_downloads | watchers_count | is_fork | prog_language    |
| ----------------------- | ------------------- | ---- | ----------------- | ------------ | ------------ | -------------------- | ------------------- | ------------ | --------- | --------------- | ----------------- | ------------------------ | ------------- | -------------- | ------------- | ------------ | -------------------- | -------- | --------- | ------------ | ------------- | -------------- | ------- | ---------------- |
| github2pandas           | 2021-02-25 13:04:06 | 7409 | 3                 | 2            | 262          | 3                    | 2021-11-18 17:03:36 | 9            | 14        | 0               | 33                | 15                       | 14            | 1              | 6336          | 61           | 43                   | True     | False     | True         | True          | True           | False   | Python           |
| github2pandas_manager   | 2021-08-22 14:47:37 | 166  | 4                 | 6            | 44           | 0                    | 2021-11-20 16:32:23 | 9            | 0         | 0               | 5                 | 0                        | 0             | 0              | 5381          | 11           | 2                    | True     | False     | True         | True          | False          | False   | Python           |
| github2pandas_notebooks | 2021-07-04 06:05:12 | 66   | 2                 | 1            | 11           | 0                    | 2021-10-11 10:04:45 | 9            | 0         | 0               | 1                 | 0                        | 0             | 0              | 4567          | 1            | 0                    | True     | False     | True         | True          | False          | False   | Jupyter Notebook |
| github2pandas_tutorials | 2021-11-20 07:34:53 | 120  | 1                 | 1            | 3            | 0                    | 2021-11-20 09:03:53 | 9            | 0         | 0               | 0                 | 0                        | 0             | 0              | 529           | 0            | 0                    | True     | False     | True         | True          | False          | False   |                  |

       {{4-5}}
```python
import pandas as pd

url="https://raw.githubusercontent.com/TUBAF-IFI-DiPiT/github2pandas_tutorials/main/examples/Demo_Project/Repositories.csv"
c=pd.read_csv(url)

```
@Pyodide.eval


## Example

--{{0}}--
My colleagues simulated student activities in a small classroom example. If you are still unsure about using Classrooms, you can find more information in the well-made tutorials.

![GitHubClassroomExample](../pics/GitHubClassroomExample.png)

### Installation



![github2pandas_manager](../pics/pypi.png)
