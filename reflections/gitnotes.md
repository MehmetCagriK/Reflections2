# VERSION CONTROL SYSTEMS: CENTRALIZED VS DECENTRALIZED

There are centralized version systems like SVN, CVS etc. And there are also
decentralized version systems like Git and Mercurial. Each has their own 
advantages and disadvantages.

## Centralized Version Control

Centralized version control systems are based on the idea that there is a
single “central” copy of your project somewhere (probably on a server), and
programmers will **commit** their changes to this central copy. Developers 
don' t have to keep copies of many versions of the project files since version 
control can talk to the central copy and retrive any changes easily.

### A Typical Centralized Version Workflow

1. Pull down any changes people made to central server.
2. Make your changes on local copy and make sure it works.
3. Commit your changes to central server, so other can see too.

## Distributed Version Control

Distributed version control systems(or dvcs in short) do not rely on central
server to store all versions of project's files(repository). Every developer
can clone a repository into their local storage including project files and
all their history. This clone has all metadata of the project repository
Author's note: This does not mean there can not be a central repository. In
fact, there will be an authorative central repository in many projects using
git. Developers can use this central repository in a centralized fashion as it
will have latest approved changes from all users.

Compared to CVS, DCVS repositories have higher storage size thanks to history
data included in them. But most of the projects are consists of text files
which users constantly update. Also most modern systems can compress files to
use even less space.

Author's note:  One of the problems that plague the current project is putting
every project related file under central SVN repository including files that
should not be put under Version Control like Visual Studio project files.
Putting these kind of files under version control bloats the central repository
and makes update/commit/checkout(SVN terms) operations far slower. The decision
of which files should be under version control is important. A question
arises in this situation: what if there were huge chunks of data files that
constantly needed to be updated. How does DVCS(Git especially) handle that?

### Advantages Of DVCS Over CVCS

1. Performing activities other than push/pull is extremely fast since they only
   need access to local storage, not remote repository.
2. Committing changes can be done locally without anyone seeing them. This
   gives users better control with local version control. Once developer is
   sure of change-set is working, they can push to remote repository.
   Note: CVCS like SVN does not allow local commits.
3. Expanding on (1): Everything other than push/pull do not need connection to
   remote repository.
4. Since everyone has a full copy of the project repository, they can share
   changes with other people for feedback without changing "central" repsitory.

### Disadvantages Of DVCS Over CVCS

1. If project contains large binary files that can not be compressed, the
   space needed to store all versions these files can accumulate quickly. This
   was the quesiton I asked in second paragraph of DVCS section
   Author's note: At this point there may be two version control systems that
   handle different file types(big ones and small ones).
   Author's note extended: After some research, I realized there is a git
   extension called Git LFS which stands for Git Large File Storage. I


2. If projects have long history, pulling/cloning the repositories takes long
   time and disk space. Because Git repositories always enlarge because of
   keeping whole history.
   Author's note: Discuss the idea of history reset. History reset or
   repository reset is simply copying a milestone version of the project into
   a new repository. Those milestone versions shall be locked in order to
   preserve history before milestone. New repository starts history from the
   milestone. So total history of the project would be seperated into multiple
   parts by creating history-less(fresh) repository for project's current
   content. But for certification purposes, these milestone versions might be
   summed for the project with total history.

