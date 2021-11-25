<!--
author:   Sebastian Zug, André Dietrich
email:    sebastian.zug@informatik.tu-freiberg.de, andre.dietrich@informatik.tu-freiberg.de,
version:  0.0.2

language: en
narrator: US English Male

import: https://github.com/LiaTemplates/Pyodide

comment:  Describes the application of github2pandas_manager for Github-Classroom projects
translation: Deutsch  translations/German.md
translation: Français translations/French.md

@playback
<script>console.log("playing @0")</script>
<script modify="false">
  let mute = false

  try {
    mute = JSON.parse(localStorage.getItem("settings")).mode == "Textbook"
  } catch (e) {
    console.log("error: ", e)
  }

  `LIASCRIPT: <audio ${mute ? "controls" : "autoplay"}><source src="@0#t=0" type="audio/mpeg"></audio>`
</script>
@end

-->

[![LiaScript](https://raw.githubusercontent.com/LiaScript/LiaScript/master/badges/course.svg)](https://liascript.github.io/course/?https://raw.githubusercontent.com/TUBAF-IFI-DiPiT/github2pandas_tutorials/main/content/GithubClassroomsBasics.md)


# Automated supervision of student activities in Github Classrooms

      --{{0}}--
Hi everyone, my name is Sebastian Zug; today I would like to introduce you to some new Python modules for analyzing student activity in a Github Classroom. The design and implementation of these packages are part of the "DiP-iT" project, a national research project at German universities. It integrates partners from Humboldt Universität zu Berlin, Otto-von-Guericke University and my affilation, TU Bergakademie Freiberg. This material was designed with Liascript; it can be found in an Github repository under the given Link.

      {{0-2}}
![github2pandas_manager](../pics/pypi.png)<!-- width="75%" -->

      {{1-2}}
@playback(../sound/1_0-3.mp3)

      {{1-2}}
_This material was designed with [Liascript](https://liascript.github.io/). Its content can be found on [Repo-Link](https://github.com/TUBAF-IFI-DiPiT/github2pandas_tutorials/blob/main/content/GithubClassroomsBasics.md). For starting the interactive mode push the button on the first slide._ ![LiaScript](https://raw.githubusercontent.com/LiaScript/LiaScript/master/badges/course.svg)

-----------------------

      --{{2}}--
But let's get started. Github Classrooms are an excellent way to teach students about the features and use of project management tools and version control. You define a task in a repository, and a clone is created for individual students or groups after their login. Afterward they work independently on the given task and commit a result. This can be automatically evaluated by pattern matching or test methods.

      {{2-3}}
@playback(../sound/1_1-3.mp3)

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

`````````

      --{{3}}--
However, depending on the size of your course, supervising student's activity becomes complex. Github Classooms provides some mechanisms, but these can only answer specific questions to a limited extent. As a result, you click from student repository to repository and evaluate the results manually.


        {{3-4}}
@playback(../sound/1_2-3.mp3)

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

--{{4}}--
Github2pandas closes this gap and allows the automatic aggregation of repository data like commits, issues or pull requests etc. The collected information is stored either in Python pandas dataframes or in csv files. The collected data is anonymized by github2pandas and can be evaluated as feedback for the instructor.

         {{4-5}}
@playback(../sound/1_3-3.mp3)

{{4-5}}
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
For our example, I uploaded the two files to the Data folder of this tutorial repository. Let us get an overview. As visible in the commandline output, we downloaded data sets of 4 projects. All of them started in 2021 but have a different history, number of contributers or code size. Even at this level, it can be guessed that an overview of the students' repositories can provide important insights into which content or features should be presented again in more depth.

       {{3-4}}
| repo_name               | creation_date       | size | contributor_count | branch_count | commit_count | commit_comment_count | last_commit_date    | labels_count | tag_count | milestone_count | pullrequest_count | pullrequest_review_count | release_count | workflow_count | readme_length | issues_count | issues_comment_count | has_wiki | has_pages | has_projects | has_downloads | watchers_count | is_fork | prog_language    |
| ----------------------- | ------------------- | ---- | ----------------- | ------------ | ------------ | -------------------- | ------------------- | ------------ | --------- | --------------- | ----------------- | ------------------------ | ------------- | -------------- | ------------- | ------------ | -------------------- | -------- | --------- | ------------ | ------------- | -------------- | ------- | ---------------- |
| github2pandas           | 2021-02-25 13:04:06 | 7409 | 3                 | 2            | 262          | 3                    | 2021-11-18 17:03:36 | 9            | 14        | 0               | 33                | 15                       | 14            | 1              | 6336          | 61           | 43                   | True     | False     | True         | True          | True           | False   | Python           |
| github2pandas_manager   | 2021-08-22 14:47:37 | 166  | 4                 | 6            | 44           | 0                    | 2021-11-20 16:32:23 | 9            | 0         | 0               | 5                 | 0                        | 0             | 0              | 5381          | 11           | 2                    | True     | False     | True         | True          | False          | False   | Python           |
| github2pandas_notebooks | 2021-07-04 06:05:12 | 66   | 2                 | 1            | 11           | 0                    | 2021-10-11 10:04:45 | 9            | 0         | 0               | 1                 | 0                        | 0             | 0              | 4567          | 1            | 0                    | True     | False     | True         | True          | False          | False   | Jupyter Notebook |
| github2pandas_tutorials | 2021-11-20 07:34:53 | 120  | 1                 | 1            | 3            | 0                    | 2021-11-20 09:03:53 | 9            | 0         | 0               | 0                 | 0                        | 0             | 0              | 529           | 0            | 0                    | True     | False     | True         | True          | False          | False   |                  |


## Application example

--{{0}}--
My colleagues simulated student activities in a classroom example prepared for this tutorial. Many thanks for this. If you are still unsure about using Classrooms, you can find more information in the well-made [tutorials](https://www.youtube.com/watch?v=xVVeqIDgCvM).

![GitHubClassroomExample](../pics/GitHubClassroomExample.png)

### Installation

--{{0}}--
Under Linux, the installation is straightforward. First, make sure that a Python 3.8 interpreter is installed on your system. Afterward, you have probably to add the Python package manager `pip` to your system, if necessary. Using this tool we install the virtual environment `pipenv`. Using a virtual environment instead of a global installation makes it easier to resolve module's dependencies. We do not need to pay attention to the numpy or pandas versions that you have installed globally. The last step is then the actual installation of `github2pandas_manager`. This step will deploy `github2pandas` and a whole set of other packages in the background. At the end of step 3, you should get the output below on your command line.

|                                                      | Commandline                            |
| ---------------------------------------------------- | -------------------------------------- |
| **Step 1 - Install package managment system**        | `sudo apt install pip3`                |
| **Step 2 - Install `pipenv` as virtual environment** | `sudo pip3 install pipenv`             |
| **Step 3 - Install `github2pandas_manager`**         | `pipenv install github2pandas_manager` |

```bash      pipenv
>> pipenv install github2pandas_manager                                       
Creating a Pipfile for this project...
Installing github2pandas_manager...
Adding github2pandas_manager to Pipfile's [packages]...
✔ Installation Succeeded
Pipfile.lock not found, creating...
Locking [dev-packages] dependencies...
Locking [packages] dependencies...
Building requirements...
Resolving dependencies...
✔ Success!
Updated Pipfile.lock (ea4d3d)!
Installing dependencies from Pipfile.lock (ea4d3d)...
  🐍   ▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉ 0/0 — 00:00:00
To activate this project's virtualenv, run pipenv shell.
Alternatively, run a command inside the virtualenv with pipenv run.
```

### Generating your GitHub-Token

       {{0-1}}
********************************************************************************

**Step 4 - Token generation**

    --{{0}}--
To access your repositories and those of your students, you have to authorize yourself to GitHub. In the browser, you do this with your password. When using a program to access GitHub content, the token substitutes the password and is sent to the server in the background. There are already many videos explaining how to generate the token; check here to know-how.

!?[GitHubToken](https://www.youtube.com/watch?v=SzrETQdGzBM)

> **You will see the token only during the generation process! Make a copy of the line immediately!**

********************************************************************************

        {{1-2}}
********************************************************************************

**Step 5 - Token storing**

     --{{1}}--
Please copy the token as an environmental variable to a hidden `.env` file. It is automatically read by the Python interpreter when you start the script. The result should look like the listing below. Dont worry, its not a valid token for my account.

```bash  .env
GITHUB_API_TOKEN ="ghp_N3nRrqNPEHt0iRhuheasdfsfas9kFxGI42GQfv"
```

> Dont worry, its not a valid token for my account.

********************************************************************************

### Preparing config file

        {{0-1}}
********************************************************************************

![GitHubClassroomExample](../pics/GitHubClassroomExample.png)

     --{{0}}--
Please remember, we named our classroom `GitHubClassroom-Demo` and called the assignment `ProgrammingTask_1`. Consequently, we see the following URLs for our simulated student repository collection. `github2pandas_manager` evaluate repository names by the already mentioned black and white patterns. Hence, we should aim at repositories with `programmingtask_1-` in name to cover all of our student's activities.

List of extracted repository URLs:

+ `https://github.com/GitHubClassroom-Demo/programmingtask_1-teamtuf`
+ `https://github.com/GitHubClassroom-Demo/programmingtask_1-team-ovgu`
+ `https://github.com/GitHubClassroom-Demo/programmingtask_1-team-avengers`

> **The corresponding white pattern, covering all repository names is `programmingtask_1-`**

********************************************************************************

            {{1-2}}
********************************************************************************

      --{{1}}--
We keep this information in the configuration file located in the main folder of our demo project. The file should look like the one shown in the listing below.

**Step 6 - Config file preparation**

```yaml  config_data_aggregation.yml
project_name: programmingtask_1

#################################################################
# Folder structure

project_folder: ./examples/

#################################################################
# Repository selection by pattern name

repo_white_pattern:
  - "programmingtask_1"

repo_black_pattern: ""    # This means empty list

#################################################################
# Content definition
content:
  - Repository
  - Version
```

********************************************************************************

> Important: Be aware that Classroom repositories are private in default configuration. For our example I changed this manually in order to provide a comprehensible example.

### Executing aggregation

        {{0-1}}
********************************************************************************

      --{{0}}--
Now we have everything together, the token is stored in an environemental variable, the configuration file is in the project folder and the installation process has been completed successfully.

Preparation completed:

+ `github2pandas_manager` installation finished
+ token stored in `.env`
+ `.yml` configuration file containing the specific white pattern

> Let's start the aggregation process!

********************************************************************************

        {{1-2}}
********************************************************************************

       --{{1}}--
Calling the script is a bit more involved because we are using the virtual environment. The following listing shows the meaning of each command line parameter. Don't miss to include the minus symbols in front of "m" and "path".

```shell Linux example
 pipenv run python3 -m github2pandas_manager -path config_data_aggregation.yml
#------ =========== ------------------------ =================================
#   ^        ^                   ^                           ^
#   |        |                   |                           |
#   |        |                   |            Location of the conifig file
#   |        |                   |            (feel free to address different folders)
#   |        |                   |    
#   |        |      Actual script reference
#   |        |      adressing __main__.py in
#   |        |      github2pandas_manager module
#   |        |
#   |   Commands exectuted  
#   |   in the virtual
#   |   environment
#   |   running python3
#   |
#Starting the
#virtual
#environement

```

********************************************************************************

       {{2-3}}
********************************************************************************

      --{{2}}--
If you have made all entries correctly and start the script, the output should look like the following. Afterward, we have 3 csv and python pandas dataframes available in our examples folder.

```shell
> pipenv run python3 -m github2pandas_manager -path config_data_aggregation.yml   
Loading .env environment variables...
3 machting repositories found.
Repository -   0 /   3 - GitHubClassroom-Demo/programmingtask_1-team-avengers (4772)
Repository -   1 /   3 - GitHubClassroom-Demo/programmingtask_1-team-ovgu (4755)
Repository -   2 /   3 - GitHubClassroom-Demo/programmingtask_1-teamtuf (4738)
Version    -   0 /   3 - GitHubClassroom-Demo/programmingtask_1-team-avengers (4721)
Found no database on provided path. Starting from scratch.
Parallel (4 processes): 100%|█████████████████████████████████| 8/8 [00:00<00:00, 18.30it/s]
Version    -   1 /   3 - GitHubClassroom-Demo/programmingtask_1-team-ovgu (4699)
Found no database on provided path. Starting from scratch.
Parallel (4 processes): 100%|█████████████████████████████████| 1/1 [00:00<00:00,  7.17it/s]
Version    -   2 /   3 - GitHubClassroom-Demo/programmingtask_1-teamtuf (4694)
Found no database on provided path. Starting from scratch.
Parallel (4 processes): 100%|█████████████████████████████████| 4/4 [00:00<00:00, 14.58it/s]
examples/programmingtask_1/Repositories.csv
examples/programmingtask_1/Edits.csv
examples/programmingtask_1/Commits.csv
Aus Maus
```

********************************************************************************

### Analysis

       {{0-1}}
********************************************************************************

       --{{0}}--
What about our results? The team avengers were highly active and submitted eight commits, but more important these colleagues discussed their collaborative work intensively using issues. The team ovgu just started the task but did not provide any new version. The commit count is still one - that means we have only the autogenerated initial commit. None of the teams uses branches, pull requests, or any of the other features. Probably we have to revise our teaching strategy related to these things.


| repo_name                       | size | contributor_count | branch_count | commit_count | pullrequest_count | release_count | workflow_count | readme_length | issues_count | issues_comment_count |
| ------------------------------- | ---- | ----------------- | ------------ | ------------ | ----------------- | ------------- | -------------- | ------------- | ------------ | -------------------- |
| programmingtask_1-team-avengers | 5    | 3                 | 1            | 8            | 0                 | 0             | 0              | 436           | 2            | 6                    |
| programmingtask_1-team-ovgu     | 0    | 1                 | 1            | 1            | 0                 | 0             | 0              | 155           | 0            | 0                    |
| programmingtask_1-teamtuf       | 2    | 3                 | 1            | 4            | 0                 | 0             | 0              | 444           | 0            | 0                    |


********************************************************************************


       {{1-2}}
********************************************************************************

       --{{0}}--
Let us focus on the significance and clearness of commit messages. Only the author `think-old-current-family` from team avengers considered the idea of self-explaining messages and added at least detailed information. By extracting the default message contents, we can supervise this aspect generally and generate statistics that can be discussed in the actual lecture.

| anonym_author             | repo_name                       | commit_message                                                                                                |
| ------------------------- | ------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| would-hot-power-president | programmingtask_1-team-avengers | Initial commit                                                                                                |
| think-old-current-family  | programmingtask_1-team-avengers | Update README.md  I feel this is a little too much demanding. I added a smiley and removed exclamation marks. |
| find-different-free-war   | programmingtask_1-team-avengers | Update README.md                                                                                              |
| find-different-free-war   | programmingtask_1-team-avengers | Update README.md                                                                                              |
| find-different-free-war   | programmingtask_1-team-avengers | Update README.md                                                                                              |
| find-different-free-war   | programmingtask_1-team-avengers | Update README.md                                                                                              |
| find-different-free-war   | programmingtask_1-team-avengers | Update README.md                                                                                              |
| think-old-current-family  | programmingtask_1-team-avengers | Update README.md  fixed incomplete sentence                                                                   |
| would-hot-power-president | programmingtask_1-team-ovgu     | Initial commit                                                                                                |
| would-hot-power-president | programmingtask_1-teamtuf       | Initial commit                                                                                                |
| call-foreign-party-result | programmingtask_1-teamtuf       | Update README.md                                                                                              |
| expect-bring-medical-lot  | programmingtask_1-teamtuf       | Create teamwork.md                                                                                            |
| expect-bring-medical-lot  | programmingtask_1-teamtuf       | Update README.md                                                                                              |

********************************************************************************

## Further steps

> ?

## Contact

Sebastian Zug, TU Bergakademie Freiberg, [sebastian.zug@informatik.tu-freiberg.de](mailto:sebastian.zug@informatik.tu-freiberg.de)

| Content         | Link                                                                              |
| --------------- | --------------------------------------------------------------------------------- |
| Team members    | * Maximilian Karl (Humboldt Universität zu Berlin )                               |
|                 | * André Dietrich (TU Bergakademie Freiberg )                                      |
|                 | * Galina Rudolf  (TU Bergakademie Freiberg )                                      |
| Project webpage | [Project Website](https://www.dip-it.ovgu.de/) (unfortunately in German)          |
| Repository      | [github2pandas_manager](https://github.com/TUBAF-IFI-DiPiT/github2pandas_manager) |

> If you have any question or ideas about new features, don't hesitate to write me a mail or to start a discussion on [Issues](https://github.com/TUBAF-IFI-DiPiT/github2pandas_manager/issues)
