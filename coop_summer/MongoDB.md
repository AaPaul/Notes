### Installation
**OS Version:Ubuntu20.04**
This method will install the latest version of it.
#### 1. Import public key
```
wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -
```
If there is an error, then input the follow commands.
```
sudo apt-get install gnupg

wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -
```

#### 2. Create a list file for MongoDB
```
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list
```

#### 3. Reload local package
```
sudo apt-get update
```

#### 4. Installation
```
sudo apt-get install -y mongodb-org
```

Use `systemctl` to start and see the status of MongoDB
```
sudo systemctl start mongod

```
Reference: https://docs.mongodb.com/manual/administration/install-on-linux/


#### 5. Set automatically start the service
```
sudo systemctl enable mongo.service
```
### Modify the MongoDB storage location
Because of the space limitaion of the root(`/`) directory, I changed the location of data storage.

Usually the default location is `/var/lib/mongodb/`

**Main steps**
```
mkdir mongodb_data
sudo chown -R mongodb:mongodb mongodb_data
sudo cp -R /var/lib/mongodb /mongodb_data
sudo systemctl restart mongodb
```


### Basic operation
#### Create DB
```
use dbname;
show tables;
```
#### Drop DB
```
use dbname;
db.dropDatabase()
```

#### Create Collection
```

```