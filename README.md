# installcelovalidator
Read it to know how to install a Celo Validator node_Baklava Testnet
Note: Tested and worked successfully on Ubuntu Server 18.04 LTS
Open your terminal, type all these commands ( if prompted Y/n please type y and enter ) ( This 5 first commands in important to backup your wallet later )
$ sudo apt-get update
$ sudo tasksel install ubuntu-mate-core
$ sudo service lightdm start
$ sudo apt install xrdp
$ sudo systemctl enable xrdp
1.Install Required Environment and Docker Repository
$ sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common build-essential libssl-dev python make gcc libssl-dev git
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
2.Install Docker CE
$ sudo apt-get update
$ sudo apt-get install containerd 
$ sudo apt-get install docker-ce docker-ce-cli
3.Setting up the runtime environment for Celo
$ sudo adduser celo
$ sudo usermod -a -G docker celo
$ sudo adduser celo sudo
$ su celo
4.Installing CeloCLI
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
$ source ~/.bashrc
$ nvm install 10 && nvm use 10
$ npm install -g @celo/celocli
5. Install Celo Blockchain Data 
$ export CELO_IMAGE=us.gcr.io/celo-testnet/celo-node:baklava
$ export NETWORK_ID=12219
$ docker pull $CELO_IMAGE
$ mkdir celo-accounts-node
$ cd celo-accounts-node
$ docker run -v $PWD:/root/.celo -it $CELO_IMAGE account new (enter your phrasephase and reconfirm it, repeat this step 1 one time and copy 2 your generated address )
$ export CELO_VALIDATOR_GROUP_ADDRESS=c1b1230bbf9590232e6d2d28c4c1c418a32339ef (replace c1b1230bbf9590232e6d2d28c4c1c418a32339ef as your 1st copied address )
$ export CELO_VALIDATOR_ADDRESS=255f0de37b99d52ad4189d06ab4f63a748b2d235 ( replace 255f0de37b99d52ad4189d06ab4f63a748b2d235 as your 2nd copied address )
$ docker run -v $PWD:/root/.celo $CELO_IMAGE init /celo/genesis.json
$ docker run -v $PWD:/root/.celo --entrypoint cp $CELO_IMAGE /celo/static-nodes.json /root/.celo/
$ docker run --name celo-accounts -it --restart always -p 8545:8545 -v $PWD:/root/.celo $CELO_IMAGE --verbosity 3 --networkid $NETWORK_ID --syncmode full --rpc --rpcaddr 0.0.0.0 --rpcapi eth,net,web3,debug,admin,personal ( required 
