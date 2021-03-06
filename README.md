# docker-strider

Semi-official docker build that does NOT ship with internal Mongo or SSH.

It contains supervisord, latest strider, its dependent modules and nodejs/npm.

You must pass in a DB_URI with a valid mongodb connection string.

## Pulling

Hosted on Quay.io. Find what to `docker pull` by checking the TAG file

## Building

Clone the project and `docker build -t <TAG> .`

## Running

Say you've created a database at MongoLab, here's how you would run it:

`docker run -e "DB_URI=mongodb://keyvan:mypass@ds041380.mongolab.com:41380/strider-testing" <TAG>`

## Linking

A compatible mongo image is included.

Following example should lead you into a side-by-side mongo/strider containers creation.

Building image `mongodb-img`:
`docker build -t mongodb-img ./mongo`

Building image `strider-img`:
`docker build -t strider-img .`

Launching a container called `database` based on previously built image `mongodb-img`:
`docker run -d --name database -i mongodb-img`
Notice that no ports were exposed.

Now we would create a container called `strider` based on previously built image `strider-img` image that is linked to previous launched `database` container. We also gonna to expose `strider:3000` into `host:80`.
```bash
docker run -d \
  --name strider \
  --link database:database \
  -e "DB_URI=mongodb://database:27017" \
  -p 80:3000 \
  strider-img
```


## Security

This image can generate an admin user with a random password. Set environment variable `GENERATE_ADMIN_USER` to use this feature.
