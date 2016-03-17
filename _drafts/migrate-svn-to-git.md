---
layout: post
title: Migrate SVN to GIT
comments: true
disqus_identifier: GUID
tags: [git, svn]
---

1- install GIT and chose CheckIn / CheckOut Style (EOL)

2- clone the SVN repo:

- (--stdLayout) suppose you are using the standard svn folder layout.
- (--authors-file=authors.txt) setup an authors.txt file to map the users.
- (--ignore-paths="packages") specify which paths you'll like to ignore. 
- (--prefix="svn/") setup a prefix (otherwise 'origin/' will be used).

git svn clone --stdlayout --authors-file=authors.txt --ignore-paths="packages" --prefix="svn/" http://server2:33333/svn/H8_Preview H8

VERIFY

"C:\Program Files (x86)\Java\jre7\bin\java.exe" -jar svn-migration-scripts.jar verify

SYNC
git svn fetch (--all)

"C:\Program Files (x86)\Java\jre7\bin\java.exe" -Dfile.encoding=utf-8 -jar ../svn-migration-scripts.jar sync-rebase

Ma di base fa:
git svn fetch
git rebase remotes/svn/trunk master

PUSH (to a remote repository)
git remote add vsts https://sidsrl.visualstudio.com/DefaultCollection/H8/_git/H8Test
git push vsts --mirror -u [--all] [provare senza -u] [--mirror]

IMPORTARE TUTTE LE BRANCH
prima di fare il push fare il checkout di tutte le branch che si desidera inviare su git (magari tutte eccetto prova)

http://stackoverflow.com/questions/10312521/how-to-fetch-all-git-branches

for remote in `git branch -r`; do git branch --track $remote; done
git svn fetch --all

now we have more branches so to keep the things in sync we must:

git rebase remotes/svn/trunk svn/master
git rebase remotes/svn/trunk master
git push vsts (--all)

now it's time to add the remote








SVN:
http://stackoverflow.com/questions/379081/track-all-remote-git-branches-as-local-branches/6300386#6300386

l'importazione crea riferimenti sbagliati alle branch, monta le diverse branch come branch remote, occorre editare il file
'packed-refs' e convertire le branch remote a branch locali, ex:

5115e2fcc662ab52852de9a4facfd93601ccf581 refs/remotes/origin/002-spartancqrs --> 5115e2fcc662ab52852de9a4facfd93601ccf581 refs/heads/002-spartancqrs

e poi fare il push.




