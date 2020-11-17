# Updating data from 2.x to 3.x

The following code snippet emulates the situation where the code is in version 3.0.0 while the data is in version 2.0.2
(this assumes that you've followed the [instructions for removing local
state](https://github.com/research-software-directory/research-software-directory/blob/3.0.0/docs/dev.md#removing-local-state)):

```shell
# give me the code state for 3.0.0 ...
git checkout 3.0.0

# ... but with sample data from version 2.0.2
git checkout 2.0.2 -- database/db-init
```

It should now be possible to upgrade the sample data as follows:

```shell
docker-compose build
docker-compose up -d
docker-compose log --follow
```

Once the Research Software Directory is up, you can check you still have the old data by visiting https://localhost/api/project/764.

In a new terminal,

```shell
# copy the migrate script to inside the running database service
docker cp data-migration/2.x-to-3.x/migrate.js $(docker-compose ps -q database):/tmp

# run the migrate script
docker-compose exec database mongo rsd /tmp/migrate.js

# update the cache
docker-compose exec harvesting python app.py resolve all
```

The data you get from the API should now be according to the 3.0.0 schema, e.g. https://localhost/api/project/764, and
all aspects of the site should now work. You should verify if everything works by doing the checks mentioned in section
[Verifying the local
installation](https://github.com/research-software-directory/research-software-directory/blob/3.0.0/docs/dev.md#verifying-the-local-installation).


## Optional: get project data

Optionally update the project data from esciencecenter.nl corporate website by doing following (continue in the
terminal from the previous section):

```shell
python3 -m virtualenv -p python3 venv3
source venv3/bin/activate
pip install -r ./harvesting/requirements.txt
source .env
export PYTHONWARNINGS="ignore:Unverified HTTPS request"
export PYTHONPATH=$PYTHONPATH:`pwd`/harvesting
python3 data-migration/2.x-to-3.x/harvest_project_info_nlesc.py

# again, update the cache
docker-compose exec harvesting python app.py resolve all
```

The data you get from the API should now include richer data for the projects, e.g. https://localhost/api/project/764,
which means that the corresponding project pages are also richer, e.g. https://localhost/projects/764. All aspects of the
site should now work. You should verify if everything works by doing the checks mentioned in section [Verifying the
local
installation](https://github.com/research-software-directory/research-software-directory/blob/3.0.0/docs/dev.md#verifying-the-local-installation).
