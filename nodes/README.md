# Umee Node - Quick Setup 

The instructions below allow you to setup an Umee node within a minute. This is super fast and easy ! Under the "settings" directory, you can customize the configuration as per your requirements.

This is a dockerized and a parameterized setup. The paramaters can be optionally be overridden with your own values. 

## How to launch the node ? 

### Step 1 -  

Clone the repo and navigate to the nodes folder and build your own node image. 

**You can also modify the go version in your build. In the Dockerfile, modify the ENV variable GO_VERSION , default is 1.20.** However, please ensure that your go version is compatible with the umeed binary you are going to use.

Here _my_umee_image_ is the name of your image you are going to build. Don't miss the **"."** in the end of the command

```
git clone https://github.com/umee-network/umee-infra.git -b main
cd umee-infra/nodes/single-node/
docker build -t my_umee_image:v1 .
```

### Step 2 -

Once the node image is successfully build, you should see something like,

```
Removing intermediate container 7326b730beb4
 ---> c5f6bc6c639a
Step 15/15 : EXPOSE 26656 9090 1317
 ---> Running in d906a9899524
Removing intermediate container d906a9899524
 ---> c14c4d11e713
Successfully built c14c4d11e713
Successfully tagged my_umee_image:v1
```

Now you can launch the node, here is a sample command below

```
docker run -dit --name umee-node --hostname umee-node \
  -e UMEE_BINARY_VERSION=v4.2.0 \
  -e CONFIG_OVERRIDE=0 \
  -e MONIKER=my-node \
  -e CHAIN_ID=umee-001 \
  -e DB_BACKEND=badgerdb \
  -e MIN_GAS_PRICE=0.1uumee \
  -e LOG_LEVEL=info \
  -p 26656:26656 \
  -p 9090:9090 \
  -p 1317:1317 \
  my_umee_image:v1 

```





