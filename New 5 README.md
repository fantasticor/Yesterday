Deploying your first Cairo 1 contract to Starknet here is what you do first
With version 0.11 rolling out on Starknet, you can now deploy your first Cairo 1 contracts on Starknet! Fantastic!

... But how ser?

Here is a very rough guide, before we get our doc in order

This is the flow
You will need to:

Clone the Cairo repository (the tool to compile your Cairo 1 contract) at a specific height
Install the latest version of Cairo-lang (the tool to interact with Starknet)
Compile your contract from Cairo to Sierra, using cairo
Declare your Sierra contract, using cairo-lang
Deploy your contract, using cairo-lang
Brag to your friends, using Twitter
Clone this repo
git clone https://github.com/starknet-edu/deploy-cairo1-demo
cd deploy-cairo1-demo
Installing the Cairo one compiler
If you're installing Cairo for the first time:

git clone https://github.com/starkware-libs/cairo/
cd cairo
git checkout 9c190561ce1e8323665857f1a77082925c817b4c
cargo build --all --release
If you already have Cairo installed:

cd cairo
git fetch && git pull
git checkout 9c190561ce1e8323665857f1a77082925c817b4c
cargo build --all --release
At this point you have Cairo installed in this repository.

Installing Cairo-lang
Go back to the root folder of the repo

cd ..
Set up a python virtual environment

python3.9 -m venv ~/cairo_venv_v11
source ~/cairo_venv_v11/bin/activate
If you had cairo-lang installed previously, uninstall it

pip3 uninstall cairo-lang
Install Cairo lang

pip3 install ecdsa fastecdsa sympy
pip3 install cairo-lang
Check that you have it installed correctly

starknet --version
Compile your contract using Cairo
You can use this smol demo contract to complete this test. It's a very basic event logger.

From your terminal, in the folder you installed Cairo in

cd cairo
cargo run --bin starknet-compile -- ../hello_starknet.cairo ../hello_starknet.json --replace-ids	
Congratulations, you have compiled your contracts from Cairo to Sierra!

Declare your contract using Cairo-lang
Setting environment variables
export STARKNET_NETWORK=alpha-goerli
export STARKNET_WALLET=starkware.starknet.wallets.open_zeppelin.OpenZeppelinAccount
Setting a new account for the tutorial
You need to make sure your starknet CLI is set up with a proper account contract, and funds. For safety, I'll create a new one just for this tutorial.

starknet new_account --account version_11
You should get your expected contract address
Send 0.1 ETH to it
Monitor the transfer transaction. Once it has passed "pending", proceed
starknet deploy_account --account version_11
Monitor the deploy transaction. Once it has passed "pending", proceed

Declaring the contract
Go back to the root folder of the tutorial then try to declare

cd ..
starknet declare --contract hello_starknet.json --account version_11
You should receive your newly declared class hash!

Troubleshooting
Ok so here I had a problem. Using declare sent back:

Error: OSError: [Errno 8] Exec format error: '~/cairo_venv_11/lib/python3.9/site-packages/starkware/starknet/compiler/v1/bin/starknet-sierra-compile'
The reason is that cairo-lang needs to compile your sierra code, and it uses an imported rust binary 'starknet-sierra-compile' for that. But in my case, the imported binary was not built for my achitecture :-(.

So, simple: I took the 'starknet-sierra-compile' from the cairo repo, that I built locally, and replaced it in the python package. Here is a command that should work:

cp cairo/target/release/starknet-sierra-compile ~/cairo_venv_v11/lib/python3.9/site-packages/starkware/starknet/compiler/v1/bin/starknet-sierra-compile
Then try again declaring. It should work!

Deploy your contract using Cairo-lang
At this point you are back in the flow you are used to. Using the class hash you received earlier:

starknet deploy --class_hash <class_hash> --account version_11
Monitor your transaction. If it fails because of fee estimation, retry. Once your transaction is accepted_on_l2.... Congratulations! You've deployed your first Cairo 1 contract!

Bragging on Twitter
Don't forget to brag! Post your deployed contract here

And don't forget to add cool functionnalities to your contract. The Starknet edu team will release soon the Cairo 1 version of Cairo-101. In the meanwhile, you can get ideas of what to do with Cairo 1 with Starklings-cairo1

Do you want to learn more?
If you are interested in

Learning how to develop cool stuff on Starknet is easier than you think
How Starknet works
What Cairo is
How Stark proofs work You should join Basecamp, our free 6 weeks training program for aspiring Starknet devs. The next two cohorts start in April and you register here for a cohort in English, and aqui para una cohort en Español.
