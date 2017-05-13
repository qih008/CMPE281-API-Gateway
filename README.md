# Create Kong API Gateway that is deployed on AWS with a 3-Node Cassandra DB Cluster

Enviroment: Ubuntu 14.04

First follow this tutorial to create single working Cassandra 
https://www.digitalocean.com/community/tutorials/how-to-install-cassandra-and-run-a-single-node-cluster-on-ubuntu-14-04

Then follow this tutorial to connect three single node cassadnra together as a 3-Node Cassandra DB Cluster
https://www.digitalocean.com/community/tutorials/how-to-run-a-multi-node-cluster-database-with-cassandra-on-ubuntu-14-04

After these two tutorials you should see the following images
![image](https://github.com/qih008/CMPE281-API-Gateway/blob/master/image/Cassandra1.png)
![image](https://github.com/qih008/CMPE281-API-Gateway/blob/master/image/Cassandra2.png)
![image](https://github.com/qih008/CMPE281-API-Gateway/blob/master/image/Cassandra3.png)

The last step is going to Kong website, and download the appropriate version, in my case, it should be Ubuntu 14.04 version
https://getkong.org/install/

scp the package to ubuntu server and use the following command:
sudo apt-get update
sudo apt-get install openssl libpcre3 procps perl
sudo dpkg -i kong-0.10.2.*.deb

change kong config file, to use cassandra as kong's database:
cd /etc/kong
sudo chmod 777 kong.conf.default
vi kong.conf.default

change database: cassandra <br />
change cassandra_contact_points = 10.0.0.217, 10.0.0.208, 10.0.0.214 (my 3 nodes ip address)
:wq

kong start --conf /etc/kong/kong.conf.default

Now you should see kong is successfully start!

Notes on how to use kong:
Post a new api
curl -i -X POST \
  --url http://10.0.0.217:8001/apis/ \
  --data 'name=San_Jose' \
  --data 'hosts=San_Jose' \
  --data 'upstream_url=http://ec2-54-219-163-27.us-west-1.compute.amazonaws.com:3000'

Check all apis
curl -i -X GET http://localhost:8001/apis/

Check just one api
curl -i -X GET http://localhost:8001/apis/{name or id}

Delete api
curl -i -X DELETE http://localhost:8001/apis/{name or id}

Update or create api
curl -i -X PUT http://localhost:8001/apis/

Here is my current apis
![image](https://github.com/qih008/CMPE281-API-Gateway/blob/master/image/apis.png)
