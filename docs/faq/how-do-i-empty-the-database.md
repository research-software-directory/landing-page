# How do I empty the database?

Obviously, this part of the documentation can lead to the **LOSS OF DATA**. Make
sure you have copies of all data that you care about. 

Assuming that ``docker images`` shows the ``rsd_`` images, and ``docker ps -a``
shows the ``rsd-`` docker containers, add the environment variables to the
terminal:

```
source rsd-secrets.env
docker-compose up
```

In a new terminal, 

```
source rsd-secrets.env
docker-compose exec database /bin/sh
```

Run the ``mongo`` command inside the ``database`` container to start the Mongo
shell there.

```
mongo
```

In Mongo shell, tell Mongo you want to use the ``rsd`` database:

```
use rsd
```

Ask for the list of collections that Mongo knows about:

```
show collections
```

For every collection that you want to delete, e.g. ``commit`` and ``project``:

```
db.commit.deleteMany({})
db.project.deleteMany({})
```

For reference, here is the Link to the Mongo shell documentation:
https://docs.mongodb.com/manual/reference/method/db.collection.deleteMany/#db.collection.deleteMany

Type Ctrl-D to exit the Mongo shell. 

After you are done making changes to the collections, you will want to update
the data in ``/database/db-init``. The data in this directory is part of the
GitHub repo and serves as sample data for when people do a ``git clone``. Now
that you have emptied some collections, that initial data needs to be updated,
as follows:

Dump the contents of the ``rsd`` database to a directory by running (still from
within the ``database`` container):
 
```
mongodump --db rsd
```

The dump files should be located at ``/dump/rsd/`` (inside the ``database``
container). Verify that the files are there and then leave the ``database``
container with:

```
exit 
```

You should now be back in the original terminal, i.e. where you ran ``source
rsd-secrets.env``. From there, copy the database dump files from inside the
container to outside the container:

```
docker cp rsd-database:/dump/rsd/ database/db-init/
```

Move the data to the appropriate place (``/database/db-init``) and delete the ``rsd`` directory:

```
cd database/db-init
mv rsd/* .
rm -r rsd
```

Update your git repository with:

```
git branch updated-data
git checkout updated-data
git add database/db-init/*
git commit
git push origin updated-data
```

After that, people who do a new ``git clone`` of your fork of the Research Software
Directory, should get the updated sample data.
 
