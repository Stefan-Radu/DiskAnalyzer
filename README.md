# DiskAnalyzer

Created a daemon that analyzes the used space on a device starting with a given path, and utilitary that iteracts with the daemon.

The utilitary is called `da` and has the following functionalities: 
- [x] Create an analysis job starting from a parent directory with a given priority
    - [x] priorities can be 1, 2, or 3 indicating the analysis order respective to the other jobs (1-low, 2-normal, 3-high)
    - [x] an analysis job for a directory which is already part of an analysis job will not create additional tasks
- [x] Discard / Delete an analysis job
- [x] Pause / Resume an analysis job
- [x] Status check for an analysis job (preparing, in progress, done)

```
   Usage: da [OPTION]... [DIR]...
   Analyze the space occupied by the directory at [DIR]
      -a, --add           analyze a new directory path for disk usage
      -p, --priority   set priority for the new analysis (works only with -a argument)
      -S, --suspend <id>  suspend task with <id>
      -R, --resume <id>  resume task with <id>
      -r, --remove <id>  remove the analysis with the given <id>
      -i, --info <id>  print status about the analysis with <id> (pending, progress, d
      -l, --list   list all analysis tasks, with their ID and the corresponding root p
      -p, --print <id>  print analysis report for those tasks that are "done"
```

Example:
```
  $> da -a /home/user/my_repo -p 3
  Created analysis task with ID ’2’ for ’/home/user/my_repo’ and priority ’high’.

  $> da -l
  ID  PRI Path  Done Status            Details
  2 *** /home/user/my_repo  45% in progress       2306 files, 11 dirs

  $> da -l
  6
  ID  PRI Path  Done Status            Details
     2 *** /home/user/my_repo  75% in progress       3201 files, 15 dirs
     
  $> da -p 2
     Path  Usage  Size Amount
     /home/user/my_repo 100%   100MB  #########################################
     |
     |-/repo1/ 31.3%  31.3MB #############
     |-/repo1/binaries/ 15.3%  15.3MB ######
     |-/repo1/src/ 5.7%   5.7MB ##
     |-/repo1/releases/ 9.0%   9.0MB  ####
     |
     |-/repo2/ 52.5%  52.5MB #####################
     |-/repo2/binaries/ 45.4%  45.4MB ##################
     |-/repo2/src/ 5.4%   5.4MB ##
     |-/repo2/releases/ 2.2%   2.2MB #
     |
     |-/repo3/ 17.2%  17.2MB ########
     [...]
     
  $> da -a /home/user/my_repo/repo2
  Directory ’home/user/my_repo/repo2’ is already included in analysis with ID ’2’
  
  $> da -r 2
  Removed analysis task with ID ’2’, status ’done’ for ’/home/user/my_repo’
  
  $> da -p 2
  No existing analysis for task ID ’2’
```
