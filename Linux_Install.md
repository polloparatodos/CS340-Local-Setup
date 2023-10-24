The following instructions are intended to assist in setting up Linux to closely match the CS340 school VM.

## Setup

### Prerequisites
* Get the CS340 files.zip [here](https://github.com/polloparatodos/CS340-Local_Setup/raw/master/CS340%20files.zip)
* Download MongoDB [here](https://repo.mongodb.org/apt/ubuntu/dists/jammy/mongodb-org/7.0/multiverse/binary-amd64/mongodb-org-server_7.0.2_amd64.deb)
* Download MongoDB Shell [here](https://downloads.mongodb.com/compass/mongodb-mongosh_2.0.2_amd64.deb?_ga=2.152946553.1902714214.1697858576-1426810728.1690164718)
* Download MongoDB Database Tools [here](https://fastdl.mongodb.org/tools/db/mongodb-database-tools-ubuntu2204-x86_64-100.9.0.deb)
* Install Jupyter Notebook and VSCode from Ubuntu Software app
* Install the following packages from the terminal:
```
sudo apt install python3-pip
pip3 install pymongo jupyter-dash dash-leaflet==0.1.9 dash==2.8.1 pandas==1.4.2
```
* Change directory in terminal to Downloads folder and install MongoDB packages
```
cd Downloads
sudo dpkg -i mongodb-org-server_7.0.2_amd64.deb mongodb-mongosh_2.0.2_amd64.deb mongodb-database-tools-ubuntu2204-x86_64-100.9.0.deb
```


### Steps

#### Unzip CS340 files and move datasets folder

```
unzip 'CS340 files.zip'
sudo mv datasets /usr/local/
```

#### Move mongodb-env.txt
```
cd mongo
mv mongodb-env.txt ~/Desktop
```

#### Change password in mongodb-env.txt
```
nano ~/Desktop/mongodb-env.txt
```
* Change the password in the file to something more secure, remember password for later step
* ctrl + s to save file
* ctrl + x to exit file

#### Start MongoDB server
```
sudo mongod --port 27017 --dbpath /var/lib/mongodb
```

#### Open second terminal to start mongosh
```
mongosh --port 27017
```
* Add privileged user, when prompted, use password you set in mongodb-env.txt
  * Note: This is different from the user you need to use in the class and should have different parameters. You will not want to use the following user there.
```
use admin
db.createUser({user: "woot", pwd: passwordPrompt(), roles: [{role: "root", db: "admin"}]})
db.adminCommand({shutdown: 1})
exit
```

#### Move mongoshrc.js
```
cd ~/Downloads/mongo
mv mongoshrc.js ~/.mongoshrc.js
```

#### Update mongod.conf
```
sudo nano /etc/mongod.conf
```
  * Find #security in the file, add the following below it.
    ```
    security:
      authorization: enabled
    ```
  * ctrl + s to save file
  * ctrl + x to exit file

#### Add mongodb-env.txt to bashrc
```
echo 'source ~/Desktop/mongodb-env.txt' >> ~/.bashrc
source ~/.bashrc
```

#### Restart MongoDB Server
* Close all terminals
* Open new terminal
```
sudo mongod --auth --port 27017 --dbpath /var/lib/mongodb
```
