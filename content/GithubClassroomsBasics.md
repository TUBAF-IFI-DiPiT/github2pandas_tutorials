<!--
author:   Sebastian Zug, André Dietrich
email:    sebastian.zug@informatik.tu-freiberg.de
version:  0.0.5

language: en
narrator: US English Male

comment:  Describes the application of github2pandas_manager for
          Github-Classroom projects

import: https://github.com/LiaTemplates/Pyodide

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

mark: <span style="background-color: @0;
                                  display: flex;
                                  width: calc(100% + 32px);
                                  margin: -16px;
                                  padding: 6px 16px 6px 16px;
                                  ">@1</span>
red:  @mark(#FF888888,@0)
green: @mark(lightgreen,@0)
gray: @mark(gray,@0)

-->

[![LiaScript](https://raw.githubusercontent.com/LiaScript/LiaScript/master/badges/course.svg)](https://liascript.github.io/course/?https://raw.githubusercontent.com/TUBAF-IFI-DiPiT/github2pandas_tutorials/main/content/GithubClassroomsBasics.md)


# Automated supervision of student programming activities in Github Classrooms

      --{{0}}--
Hi everyone, Sebastian Zug here from TU Bergakademie Freiberg, introducing you to some new Python modules for analyzing student activity in a Github Classroom. The team in Freiberg designed and implemented these packages with our partners from two other German universities: Humboldt Universität zu  Berlin and Otto-von-Guericke University from Magdeburg as part of the "DiP-iT" national research project. We generated this material with Liascript. The content as well as the package repositories you can find under the given links. Let's start with some background information and concepts. Afterwards, we jump into a practical examples.

![github2pandas_manager](../pics/frontpage.png)

----------------------------------------------------------

<table>
<tr>
<td style="width:30%">
![Logoanager](../pics/Logo-DiP-iT.png)<!-- width="65%" -->
</td>
<td>
+ Technische Universität Bergakademie Freiberg
+ Humboldt Universität zu Berlin
+ Otto-von-Guericke Universität Magdeburg
</td>
</tr>
</table>

--------------------------------------------------------


__André Dietrich, Sebastian Zug, 2021__

-----------------------------------------------------------


@playback(../sound/1_0.mp3)

## Concepts

    --{{0}}--
Github Classrooms are an excellent way to teach students
about the features and use of project management tools and version control. You
define a task in a repository, and a clone is created for individual students or
groups after their login. Afterward they work independently on the given task
and commit a result. This can be automatically evaluated by pattern matching or
test methods.

     {{0-1}}
@playback(../sound/2_0-2.mp3)

     {{0-1}}
`````````

 Teachers repository                              Student repositories
 --------------------                             --------------------

                                                         .-----------.
                                                  ╔══════| Student 1 |══════╗
                                        enroll    ║      '-----------'      ║
                                      +---------- ║ Digital Systems 2021    ║
                                      | +-------> ║                         ║
+------------------+                  v | clone   ║ import numpy as np      ║
| # Digital Systems|\          .-,(   ),-.        ║ ...                     ║
| (Sprint 2021)    +-+      .-(  Github   )-.     ╚═════════════════════════╝
|                    | --> (    Classroom    )
| ## Task 1          |      '-( Assignment)-'            .-----------.
|                    |         '-.(   ).-'        ╔══════| Student 2 |══════╗
| + Implement ...    |              ^ ^ |         ║      '-----------'      ║
+--------------------+              | | | clone   ║ Digital Systems 2021    ║
                                    | | +-------> ║                         ║
                                    | +---------- ║"#"include<stdio.h>      ║
                                    +-------------║ ...                     ║
                                  commit solution ╚═════════════════════════╝

                                                            .....                                         .
`````````

    --{{1}}--
However, depending on the size of your course, supervising student's activity
becomes complex. Github Classooms provides some mechanisms for exploring repository progress, but these can only
answer specific questions to a limited extent. As a result, you click from
student repository to repository and aggregate the results manually.


     {{1-2}}
***************************************************************************

@playback(../sound/2_1-2.mp3)


`````````
 Teachers repository                              Student repositories
 --------------------                             --------------------

                                                         .-----------.
                                                  ╔══════| Student 1 |══════╗
                                        enroll    ║      '-----------'      ║
                                      +---------- ║ Digital Systems 2021    ║
                                      | +-------> ║                         ║
+------------------+                  v | clone   ║ import numpy as np      ║
| # Digital Systems|\          .-,(   ),-.        ║ ...                     ║
| (Sprint 2021)    +-+      .-(  Github   )-.     ╚═════════════════════════╝
|                    | --> (    Classroom    )
| ## Task 1          |      '-( Assignment)-'            .-----------.
|                    |         '-.(   ).-'        ╔══════| Student 2 |══════╗
| + Implement ...    |              ^ ^ |         ║      '-----------'      ║
+--------------------+              | | | clone   ║ Digital Systems 2021    ║
                                    | | +-------> ║                         ║
                                    | +---------- ║"#"include<stdio.h>      ║
                                    +-------------║ ...                     ║
                                                  ╚═════════════════════════╝

                                                            .....                                         .
                                                 |                           |
+-------------------------------+                .-------------+-------------.
| Used features:                |                              |
|    | issues | actions | ... | | <----------------------------+
| A  |    X   |    x    |     | |        manual process
| B  |    x   |         |     | |
+-------------------------------+
Report on activities
`````````

> __Questions:__
>
> + How many students used feature X?
> + At which level teams worked collaboratively or cooperatively together?
> + ...
***************************************************************************

    --{{2}}--
Github2pandas closes this gap and allows the automatic aggregation of repository
data like commits, issues or pull requests etc. The collected information is
stored either in Python pandas dataframes or in csv files. The collected data is
anonymized by github2pandas and can be evaluated as feedback for the instructor.

     {{2-3}}
@playback(../sound/2_2-2.mp3)

     {{2-3}}
`````````
 Teachers repository                              Student repositories
 --------------------                             --------------------

                                                         .-----------.
                                                  ╔══════| Student 1 |══════╗
                                        enroll    ║      '-----------'      ║
                                      +---------- ║ Digital Systems 2021    ║
                                      | +-------> ║                         ║
+------------------+                  v | clone   ║ import numpy as np      ║
| # Digital Systems|\          .-,(   ),-.        ║ ...                     ║
| (Sprint 2021)    +-+      .-(  Github   )-.     ╚═════════════════════════╝
|                    | --> (    Classroom    )
| ## Task 1          |      '-( Assignment)-'            .-----------.
|                    |         '-.(   ).-'        ╔══════| Student 2 |══════╗
| + Implement ...    |              ^ ^ |         ║      '-----------'      ║
+--------------------+              | | | clone   ║ Digital Systems 2021    ║
                                    | | +-------> ║                         ║
+--------------------+              | +---------- ║"#"include<stdio.h>      ║
| # Digital Systems  |\             +-------------║ ...                     ║
| Report generator   +-+                          ╚═════════════════════════╝
|                      | <----------------+
| import github2pandas | ---+             |                  .....                                         .
+----------------------+    |             |      |                           |
                            v             |      .-------------+-------------.
+-------------------------------+         |                    |
| Used features:                |         +--------------------+
|    | issues | actions | ... | |           script based aggreation
| A  |    X   |    x    |     | |           & analysis
| B  |    x   |         |     | |
+-------------------------------+
Report on activities
`````````


## Implementation

    --{{0}}--
The sequence diagram shows the aggregation phase on an abstract level. It uses
the two Python modules prepared by the team. github2pandas implements the actual
data aggregation from one repository, while github2pandas_manager coordinates
the survey across multiple repositories. The user specifies his requirements in a yaml
file and passes it when calling the program. The demand definition may contain lists of repository but also patterns and search queries. In addition, the user defines
what information - commits, issues, workflows, etc. - should be collected.
Finally, Github2pandas_manager identifies the relevant repositories and collects
the desired data set for each of them.  In the end, the information is saved as
a pandas data frame or csv file.

       {{0}}
@playback(../sound/3_0-3.mp3)

    {{0-1}}
![Workflow](https://www.plantuml.com/plantuml/svg/fLDDYnD14BtthoZmeg1py9P0P4LPzR0NGJmkbUbAfxG_nghg3OlutqqFSKRSsMYmOPXYfjvxLU_HLseeLbDqs5iH-AGaRa0nxdd0R13OzdNxybXxrDk46SEv3kVHS8jAfy_mPBMwlbwjdDjiu2CDHTcAtCFh48G26fSCcurpJHTUl5gMMuFCo07DIBB2q-uUKtpc5Y1dkG9b4ZG2eM-LrFv6i0OJp9hO_a2SqS2i1vAf_pz6dFQEh3Qw-1OD7_WNInd6xjk-r6nWd4WT7C_uv_kRaXARFeSFgfMExyz5lkvYEHpBhkj-E3YTN9eiXxr1sJqstqt9RIZE0OGIScxLExRtTVjhPuZS1Bk9-DyyM4zupfxls5UCuD5mcMV6pq31mnBYWTGPnkMjrOhGI0sSOP3oXNg3NOcUP2IZx5rxBesx3XwD07_BTC_QKfy3lo49rAA-c3sDoDdEoI3OSIHzdA_ToSbMyjFcg73ogWZqUdVYkQBiQue_0G00)

   --{{1}}--
I took the following exemplary configuration file from the
package documentation. Starting in line 1 it references the
project name and the data root folder. In this project the aggregation selects
relevant repositories according to white and black patterns applied on repo
names. Multiple entries can be included in both lists, whereby black-list
entries are excluded from aggregation. In the example, the issue list and a complete
overview of the versions is in focus of the project.

    {{1}}
@playback(../sound/3_1-3.mp3)

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
Running github2pandas_manager with this configuration creates two datasets that
we can analyze afterwards. We are going to talk about the installation process
in several minutes. As you can see, we run two aggregation loops for Repository
data as well as mentioned issue data.

     {{2}}
@playback(../sound/3_2-3.mp3)

    {{2-4}}
```
> pipenv run python3 -m github2pandas_manager -path config_data_aggregation.yml
Loading .env environment variables...
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
For our example, I uploaded the two resulting exemplary files into the data folder of our tutorial
repository. Take a closer view to the demo project files. As visible in the commandline output, we
downloaded data sets of 4 projects. All of them started in 2021 but have a
different history, number of contributors or code size. Even at this level, the overview of the students' repositories can provide
important insights. Which features are not or not effectively used and should be presented in the exercise again.

     {{3}}
@playback(../sound/3_3-3.mp3)

    {{3-4}}
<!-- data-type="none" -->
| repo_name               | creation_date       | size | contributor_count | branch_count | commit_count | commit_comment_count | last_commit_date    | labels_count | tag_count | milestone_count | pullrequest_count | pullrequest_review_count | release_count | workflow_count | readme_length | issues_count | issues_comment_count | has_wiki | has_pages | has_projects | has_downloads | watchers_count | is_fork | prog_language    |
| ----------------------- | ------------------- | ---- | ----------------- | ------------ | ------------ | -------------------- | ------------------- | ------------ | --------- | --------------- | ----------------- | ------------------------ | ------------- | -------------- | ------------- | ------------ | -------------------- | -------- | --------- | ------------ | ------------- | -------------- | ------- | ---------------- |
| github2pandas           | 2021-02-25 13:04:06 | 7409 | 3                 | 2            | 262          | 3                    | 2021-11-18 17:03:36 | 9            | 14        | 0               | 33                | 15                       | 14            | 1              | 6336          | 61           | 43                   | True     | False     | True         | True          | True           | False   | Python           |
| github2pandas_manager   | 2021-08-22 14:47:37 | 166  | 4                 | 6            | 44           | 0                    | 2021-11-20 16:32:23 | 9            | 0         | 0               | 5                 | 0                        | 0             | 0              | 5381          | 11           | 2                    | True     | False     | True         | True          | False          | False   | Python           |
| github2pandas_notebooks | 2021-07-04 06:05:12 | 66   | 2                 | 1            | 11           | 0                    | 2021-10-11 10:04:45 | 9            | 0         | 0               | 1                 | 0                        | 0             | 0              | 4567          | 1            | 0                    | True     | False     | True         | True          | False          | False   | Jupyter Notebook |
| github2pandas_tutorials | 2021-11-20 07:34:53 | 120  | 1                 | 1            | 3            | 0                    | 2021-11-20 09:03:53 | 9            | 0         | 0               | 0                 | 0                        | 0             | 0              | 529           | 0            | 0                    | True     | False     | True         | True          | False          | False   |                  |


## Application example

   --{{0}}--
My colleagues simulated student activities in a classroom example prepared for
this tutorial. Many thanks for this. If you are still unsure about using
Classrooms, you can find more information in the well-made
[tutorials](https://www.youtube.com/watch?v=xVVeqIDgCvM).

     {{0}}
@playback(../sound/4_0.mp3)

![GitHubClassroomExample](../pics/GitHubClassroomExample.png)<!-- width="75%" -->


### Installation

   --{{0}}--
Under Linux, the installation process is straightforward. For Windows please read the instructions in the README file of our project. In both cases, make sure that a Python
3.8 interpreter is installed on your system. Depending on your system configuration step one and two in the tabular are probably not necessary. The Python package manager `pip` and the virtual environment `pipenv` are used to automate the installation process. By using a virtual environment instead
of a global installation it easier to resolve our module's dependencies. The last step is then the actual installation of
`github2pandas_manager`. This step will deploy `github2pandas` and a whole set
of other packages in the background. At the end of step 3, you should get the
output below on your command line.

     {{0}}
@playback(../sound/5_0.mp3)

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
To access your repositories and those of your students, you have to authorize
yourself to GitHub. In the browser, you do this with your password. When using a
program to access GitHub content, the token substitutes the password and is sent
to the server in the background. There are already many tutorials explaining how to
generate the token; I have embedded one example here.

     {{0}}
@playback(../sound/6_0-1.mp3)

!?[GitHubToken](https://www.youtube.com/watch?v=SzrETQdGzBM)

> **You will see the token only during the generation process! Make a copy of the line immediately!**

********************************************************************************

    {{1-2}}
********************************************************************************

**Step 5 - Token storing**

   --{{1}}--
Please copy the token as an environmental variable to a hidden `.env` file. It
is automatically read by the Python interpreter when you start the script. The
result should look like the listing below. Dont worry, its not a valid token for
my account.

    {{1}}
@playback(../sound/6_1-1.mp3)

```bash  .env
GITHUB_API_TOKEN ="ghp_N3nRrqNPEHt0iRhuheasdfsfas9kFxGI42GQfv"
```

> Dont worry, its not a valid token for my account.

********************************************************************************


### Preparing config file

    {{0-1}}
********************************************************************************

![GitHubClassroomExample](../pics/GitHubClassroomExample.png)<!-- width="50%" -->

   --{{0}}--
Please remember, we named our classroom `GitHubClassroom-Demo` and called the
assignment `ProgrammingTask_1`. Consequently, we see the following URLs for our
simulated student repository collection. `github2pandas_manager` evaluate
repository names by the already mentioned black and white patterns. Hence, we
should aim at repositories with `programmingtask_1-` in name to cover all of our
student's activities.

     {{0}}
@playback(../sound/7_0-1.mp3)

List of extracted repository URLs:

+ `https://github.com/GitHubClassroom-Demo/programmingtask_1-teamtuf`
+ `https://github.com/GitHubClassroom-Demo/programmingtask_1-team-ovgu`
+ `https://github.com/GitHubClassroom-Demo/programmingtask_1-team-avengers`

> **The corresponding white pattern, covering all repository names is `programmingtask_1-`**

> Important: Be aware that Classroom repositories are private in default
> configuration. For our example I changed this manually in order to provide a
> comprehensible example.

********************************************************************************

    {{1-2}}
********************************************************************************

   --{{1}}--
We keep this information in the configuration file located in the main folder of
our demo project. The file should look like the one shown in the listing below.

     {{1}}
@playback(../sound/7_1-1.mp3)

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

### Executing aggregation

    {{0-1}}
********************************************************************************

   --{{0}}--
Now we have everything together, the token is stored in an environemental
variable, the configuration file is in the project folder and the installation
process has been completed successfully.

     {{0}}
@playback(../sound/8_0-2.mp3)

Preparation completed:

+ `github2pandas_manager` installation finished
+ token stored in `.env`
+ `.yml` configuration file containing the specific white pattern

> Let's start the aggregation process!

********************************************************************************


    {{1-2}}
********************************************************************************

   --{{1}}--
Calling the actual script is a bit more complex because we are using the virtual
environment. The following listing shows the meaning of each command line
parameter. Don't miss to include the minus symbols in front of "m" and "path". pipenv starts our python3 example in the virtual environment. We call the main file of the github2pandas_manager package and pass our configuration file under the given path.

    {{1}}
@playback(../sound/8_1-2.mp3)

```shell Linux example
 pipenv run python3 -m github2pandas_manager -path config_data_aggregation.yml
#------ =========== ------------------------ =================================
#   ^        ^                   ^                           ^
#   |        |                   |                           |
#   |        |                   |            Location of the config file
#   |        |                   |            (feel free to address different folders)
#   |        |                   |
#   |        |      Actual script reference
#   |        |      addressing __main__.py in
#   |        |      github2pandas_manager module
#   |        |
#   |   Commands executed
#   |   in the virtual
#   |   environment
#   |   running python3
#   |
#Starting the
#virtual
#environment
```

********************************************************************************


    {{2-3}}
********************************************************************************

   --{{2}}--
If you preparation was successful and you started the script, the output should
look like the following listing. Afterward, we have 3 csv and python pandas dataframes
available in our examples folder.

    {{2}}
@playback(../sound/8_2-2.mp3)

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
What about our results? The team avengers were highly active and submitted eight
commits, but more important these colleagues discussed their collaborative work
intensively using issues. The team ovgu just started the task but did not
provide any new version. The commit count is still one - that means we have only
the auto-generated initial commit. None of the teams uses branches, pull
requests, or any of the other features. Probably we have to revise our teaching
strategy related to these things.

    {{0}}
@playback(../sound/9_0-1.mp3)

<!--data-type="none"-->
| repo_name              | team-avengers | team-ovgu | teamtuf |
| ---------------------- | ------------- | --------- | ------- |
| `size`                 | 5             | 0         | 2       |
| `contributor_count`    | 3             | @red(1)   | 3       |
| `branch_count`         | 1             | 1         | 1       |
| `commit_count`         | @green(8)     | 1         | 4       |
| `pullrequest_count`    | 0             | 0         | 0       |
| `release_count`        | 0             | 0         | 0       |
| `workflow_count`       | 0             | 0         | 0       |
| `readme_length`        | 436           | 155       | 444     |
| `issues_count`         | @green(2)     | 0         | 0       |
| `issues_comment_count` | @green(6)     | 0         | 0       |


********************************************************************************


     {{1}}
********************************************************************************

   --{{1}}--
Let us focus on a more specific topic and evaluate the significance and clearness of commit messages. For this purpose we explore the version.csv file. It summarizes all information about the size, authorship, etc. from all teams and references corresponding edits in specific files. In this way you can investigate the differences between two versions in detail. For sake of simplicity we extracted here the commit messages only. You can see the anonymized author names summarizing four auto-generated words instead of an hash value or a complex number code. In this way we hide student names but provide a high recognisability. Only the
author `think-old-current-family` from team avengers considered the idea of
self-explaining messages and added individual information for each commit. By extracting
the default message contents, we can supervise this aspect generally and
generate statistics that can be discussed in the actual lecture.

    {{1}}
@playback(../sound/9_1-1.mp3)

| anonym_author             | repo_name                       | commit_message                                                                                                |
| ------------------------- | ------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| would-hot-power-president | programmingtask_1-team-avengers | Initial commit                                                                                                |
| @green(think-old-current-family)  | programmingtask_1-team-avengers | Update README.md  I feel this is a little too much demanding. I added a smiley and removed exclamation marks. |
| find-different-free-war   | programmingtask_1-team-avengers | Update README.md                                                                                              |
| find-different-free-war   | programmingtask_1-team-avengers | Update README.md                                                                                              |
| find-different-free-war   | programmingtask_1-team-avengers | Update README.md                                                                                              |
| find-different-free-war   | programmingtask_1-team-avengers | Update README.md                                                                                              |
| find-different-free-war   | programmingtask_1-team-avengers | Update README.md                                                                                              |
| @green(think-old-current-family)  | programmingtask_1-team-avengers | Update README.md  fixed incomplete sentence                                                                   |
| would-hot-power-president | programmingtask_1-team-ovgu     | Initial commit                                                                                                |
| would-hot-power-president | programmingtask_1-teamtuf       | Initial commit                                                                                                |
| call-foreign-party-result | programmingtask_1-teamtuf       | Update README.md                                                                                              |
| expect-bring-medical-lot  | programmingtask_1-teamtuf       | Create teamwork.md                                                                                            |
| expect-bring-medical-lot  | programmingtask_1-teamtuf       | Update README.md                                                                                              |

********************************************************************************


## Further steps for the project

     --{{0}}--
What are the next steps? We have to extend the data aggregation process by integrating additional elements and improve the robustness of the software. Secondly, it is necessary to adapt and complete the documentation of our project on all levels. And as a last point, we are going to include additional tutorials for data evaluation. As a medium-term goal we aim at an automated feedback systems for our students, tutors and lectures based on github2pandas_manager.

    {{0}}
@playback(../sound/10_0.mp3)

- Improving stability of the aggregations processes
- Extending the documentation
- Providing more tutorials especially on data exploration and classification based on collected data sets

## Contact

      --{{0}}--
If you are interested to support this open source project as a developer or as a lecturer with new ideas or requirements don't hesitate to contact us. Additionally, we are interested in your experiences when using github2pandas_manager. Looking forward to your feedback or questions!

    {{0}}
@playback(../sound/11_0.mp3)

> Sebastian Zug, TU Bergakademie Freiberg, [sebastian.zug@informatik.tu-freiberg.de](mailto:sebastian.zug@informatik.tu-freiberg.de)

|          | References                                                                              |
| --------------- | --------------------------------------------------------------------------------- |
| Team members    | <div>     <ul style="margin-left: 20px;">
      <li>Maximilian Karl (Humboldt Universität zu Berlin)</li>
      <li>Galina Rudolf  (TU Bergakademie Freiberg)</li>
      <li>Mezekr Weldu  (TU Bergakademie Freiberg)</li>
      <li>Galina Rudolf  (TU Bergakademie Freiberg)</li>
    </ul></div>                              |
| Project webpage | [Project Website](https://www.dip-it.ovgu.de/) (unfortunately in German only)     |
| Repository      | [github2pandas_manager](https://github.com/TUBAF-IFI-DiPiT/github2pandas_manager) |

> If you have any question or ideas about new features, don't hesitate to write
> me a mail or to start a discussion on
> [Issues](https://github.com/TUBAF-IFI-DiPiT/github2pandas_manager/issues)
