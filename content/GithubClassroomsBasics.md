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

       {{0}}
@playback(../sound/3_0-3.mp3)

    {{0-1}}
![Workflow](https://www.plantuml.com/plantuml/svg/fLDDYnD14BtthoZmeg1py9P0P4LPzR0NGJmkbUbAfxG_nghg3OlutqqFSKRSsMYmOPXYfjvxLU_HLseeLbDqs5iH-AGaRa0nxdd0R13OzdNxybXxrDk46SEv3kVHS8jAfy_mPBMwlbwjdDjiu2CDHTcAtCFh48G26fSCcurpJHTUl5gMMuFCo07DIBB2q-uUKtpc5Y1dkG9b4ZG2eM-LrFv6i0OJp9hO_a2SqS2i1vAf_pz6dFQEh3Qw-1OD7_WNInd6xjk-r6nWd4WT7C_uv_kRaXARFeSFgfMExyz5lkvYEHpBhkj-E3YTN9eiXxr1sJqstqt9RIZE0OGIScxLExRtTVjhPuZS1Bk9-DyyM4zupfxls5UCuD5mcMV6pq31mnBYWTGPnkMjrOhGI0sSOP3oXNg3NOcUP2IZx5rxBesx3XwD07_BTC_QKfy3lo49rAA-c3sDoDdEoI3OSIHzdA_ToSbMyjFcg73ogWZqUdVYkQBiQue_0G00)

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

[tutorials](https://www.youtube.com/watch?v=xVVeqIDgCvM).

     {{0}}
@playback(../sound/4_0.mp3)

![GitHubClassroomExample](../pics/GitHubClassroomExample.png)<!-- width="75%" -->


### Installation

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

     {{0}}
@playback(../sound/6_0-1.mp3)

!?[GitHubToken](https://www.youtube.com/watch?v=SzrETQdGzBM)

> **You will see the token only during the generation process! Make a copy of the line immediately!**

********************************************************************************

    {{1-2}}
********************************************************************************

**Step 5 - Token storing**

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

    {{0}}
@playback(../sound/10_0.mp3)

- Improving stability of the aggregations processes
- Extending the documentation
- Providing more tutorials especially on data exploration and classification based on collected data sets

## Contact

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
