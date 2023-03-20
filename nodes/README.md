# Umee Node - Quick Setup 

The instructions below allow you to setup an Umee node within a minute. This is super fast and easy ! Under the "settings" directory, you can customize the configuration as per your requirements.

This is a dockerized and a parameterized setup. The paramaters can be optionally be overridden with your own values. 

## How to launch the node ? 

### Step 1 -  

Clone the repo and navigate to the nodes folder and build your own node image. 

**You can also modify the go version in your build. In the Dockerfile, modify the ENV variable GO_VERSION , default is 1.20.** However, please ensure that your go version is compatible with the umeed binary you are going to use.

Here _my_super_node_ is the name of your image you are going to build. Don't miss the **"."** in the end of the command

```
git clone https://github.com/umee-network/umee-infra.git -b main
cd umee-infra/nodes/single-node/
docker build -t my_super_node:v1 .
```

### Step 2 -







