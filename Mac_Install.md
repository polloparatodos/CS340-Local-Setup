The following instructions are intended to assist in getting setup with MongoDB locally with CS340 in mind

Note: I used macOS Ventura 13.5.2 (22G91), but the instructions should still work for you. I tested with macOS 14 and there are some dragons, so be warned.

## Setup

### Prerequisites

* Install MongoDB via Brew by following the instructions listed [here](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-os-x/#install-mongodb-community-edition)
* Install the following packages:
```
brew install python3
pip3 install jupyter-dash dash-leaflet==0.1.9 dash==2.8.1 pandas==1.4.2
```


### Steps

#### Clean the existing datasets before beginning

`sudo rm -rv /usr/local/datasets`

#### Put directories/files in place

```
git clone git@github.com:polloparatodos/CS340-Local_Setup.git ~/Desktop/MongoDB_Setup # Or somewhere else
cd $_
sudo mv datasets /usr/local/
cd ~/Desktop/MongoDB_Setup/mongo
# move mongodb files
mv mongodb-env.txt ~/Desktop
mv mongoshrc.js ~/.mongoshrc.js
```

#### start mongo server (no auth)
`brew services start mongodb-community@7.0`

#### Create a shell script with MongoDB commands

```
echo '#!/bin/bash' > mongosh_script.sh
echo 'mongosh --port 27017 << EOF' >> mongosh_script.sh
echo 'use admin' >> mongosh_script.sh
# Change password here and in mongodb-env file, located in mongo folder.
echo 'db.createUser({user: "<changeme>", pwd: "<changeme>", roles: [{role: "root", db: "admin"}]})' >> mongosh_script.sh
echo 'db.adminCommand({shutdown: 1})' >> mongosh_script.sh
echo 'exit' >> mongosh_script.sh
echo 'EOF' >> mongosh_script.sh
```

#### Make the shell script executable
`chmod +x mongosh_script.sh`

#### Start mongosh in a new terminal using the shell script
`./mongosh_script.sh`

#### Update MongoDB config to enable authentication
`sudo sed -i 's/#security:/security:\n  authorization: enabled/' /etc/mongod.conf`

#### start mongo server (with auth)
`brew services restart mongodb-community@7.0`
