# MongoDB and Security

<p>MongoDB provides various features, such as authentication, access control, encryption, to secure your MongoDB deployments. Some key security features include:<p>

* Authentication
* Authorization
* TLS/SSL
* Encryption


## Starting mongo servers with authorization

<br/>

## Create a user

<br/>

## with admin privileges

<br/>

```sh
> use admin

> db.createUser({user: "user", pwd: "password", roles: ["userAdminAnyDatabase"]})
```

<br/>

## with restricted privileges

<br/>

```sh
> use shop

> db.createUser({user: "appdev", pwd: "appdevpwd", roles: ["readWrite"]})

> db.auth("appdev", "appdevpwd")

> db.products.insertOne({name: "Book"})
```

<br/>

## Update a user

<br/>

```sh
> db.updateUser("appdev", {roles: ["readWrite", {role: "readWrite", db: "blog"}]})

> db.blog.insertOne({title: "Test blog post"})
```

<br/>

## Logging to the server with auth

<br/>

```sh
> db.auth("user", "password")
```

<br/>

## Adding SSL Transport Encryption

<br/>

```sh
> **generate a self certified certificate and create a mongodb.pem file**

openssl req -newkey rsa:2048 -new -x509 -days 365 -nodes -out mongodb-cert.crt -keyout mongodb-cert.key

cat mongodb-cert.key mongodb-cert.crt >mongodb.pem

> **start mongo server with tsl encryption**
> mongod --tlsMode requireTLS --tlsCertificateKeyFile /home/mdlima/.certs/mongodb.pem --dbpath /home/mdlima/

> **start mongo client with tsl encryption**
> mongo --tls --tlsCAFile /home/mdlima/.certs/mongodb.pem --host localhost
```