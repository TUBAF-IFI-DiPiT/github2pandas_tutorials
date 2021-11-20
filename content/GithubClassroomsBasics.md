<!--
author:   Sebastian Zug, André Dietrich
email:    sebastian.zug@informatik.tu-freiberg.de, andre.dietrich@informatik.tu-freiberg.de,
version:  0.0.1

language: en
narrator: US English Male

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

hier gehts weiter
