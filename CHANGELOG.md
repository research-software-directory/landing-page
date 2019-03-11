# 1.1.0

- added a simple OAI-PMH interface to allow harvesting of metadata about the 
items in the Research Software Directory in datacite4 format. The interface
implementation is not complete; at the moment, the OAI-PMH verbs ``ListRecord``
and ``GetRecords`` are implemented (but without any time based slicing such as
using ``from`` or ``until``, and without subsetting based on ``set``)
- added a service that visualizes the state of the Research Software Directory 
instance as simple graphs, e.g. histograms of how many contributors there are 
per software package.
- no longer using git submodules, this should make installing a lot easier. 
Having a monolithic repo also means that it is easier to see diffs between an 
'upstream' instance of the Research Software Direcotry, and one of its
descendants. Finally, archiving of the software via Zenodo works better for a
monorepo than for a repo with multiple submodules.
- bugfixes
- added a lot of documentation

# 1.0.0

First stable release.

Names below refer to repositories within the https://github.com/research-software-directory/ GitHub organization.

submodules SHAs:
```
 6856e70204eac7e0bb5b63cd0c391b13917966d9 admin (v1-140-g6856e70)
 fa3e0e62033dba864437af8c3b929e639428a4ac auth-github (heads/master)
 97c26c5f2825396f03fd029c32c3e21404c3b3a5 backend (v1-122-g97c26c5)
 f553e6d076471eab390b44d39c915e48f3b27ba4 db-dump (heads/master)
 1d82f242c3b2d15973b50f9a4096a16e72bcb0be frontend (v1-79-g1d82f24)
 362da73a4d594972318d5d11e3d6fb81402a46da readthedocs (heads/master)
 b4a99564651a547488c3a00efbb0b0ccbd75a7fe tasks-nlesc (heads/master)
```

# 0.0.0

First release, mostly testing the (Zenodo| GitHub) infrastructure at this point.

