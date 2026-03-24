<div align="left">

##   **Introduction📔**

Arc is a Layer 1 blockchain by Circle built for stablecoin-based finance, and it’s EVM-compatible, so if you’ve used Ethereum or similar chains it will feel familiar it’s currently live on public testnet with mainnet expected later in 2026 and one key thing to know is that gas fees are paid in USDC instead of ETH, meaning you won’t need test ETH since you’ll get testnet USDC directly from Circle’s faucet.

</div>
<div align="center">

#  👨🏻‍💻 **Deploy Your First Smart Contract on Arc Testnet** 👨🏻‍💻 

<img width="1536" height="620" alt="ChatGPT Image Mar 21, 2026, 06_28_42 PM" src="https://github.com/user-attachments/assets/aee0172d-af3a-42ec-8514-cba435b2f0ed" />

</div>

<div align="center">

## **Getting Started (Windows)⚙️**

</div>

You'll need WSL (Windows Subsystem for Linux) to follow this guide. If you don't have it yet, open PowerShell as Administrator and run:
```
wsl --install
```
* >Restart your PC after installation, then open "Ubuntu" from your Start menu. That gives you a Linux terminal inside Windows.

### Install Foundry:
```
curl -L foundry.paradigm.xyz | bash
```
* >Reload your shell config:
```
source ~/.bashrc
```
* >Install the Foundry tools (forge, cast, anvil, chisel):
```
foundryup
```
* >Tip: If you use VS Code, type 'code .' inside your WSL terminal to open any folder directly in VS Code. You may need to install the "WSL" extension in VS Code first.

* >From here on, every command is the same whether you're on Windows or Mac. Just make sure you're running them in your WSL Ubuntu terminal (Windows) or terminal (Mac).

* >I'm on windows personally, so I'll cover the windows setup first. Mac instructions follow right after. Once Foundry is installed, every command from Step 1 onward is exactly the same regardless of your OS.

## Step 1: Initialize a New Solidity Project
```
forge init hello-arc && cd hello-arc
```
* >This creates a new project folder called "hello-arc" and moves you into it. Open it in your code editor.

Your control panel should look like this 👇 
IMAGE

## Step 2: Set Up Your Environment Variables
* >Create a .env file in the root of the "hello-arc" folder. You can do this manually in your editor or run:
```
touch .env
```
* >Open it:
```
nano .env
```
* >Paste the following inside:
```
ARC_TESTNET_RPC_URL="https://rpc.testnet.arc.network"
```
* >Make sure you include the https:// prefix. Without it, your deployment commands will fail.
Save and exit nano by pressing `Ctrl + X`, then `Y`, then `Enter`.

## Step 3: Write the Smart Contract
* >First, remove the default Counter contract that Foundry generates:
```
rm src/Counter.sol
```
* >Create your new contract file:
```
touch src/HelloArchitect.sol
```
* >Open it:
```
nano src/HelloArchitect.sol
```
* >Paste this code inside:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.30;

contract HelloArchitect {
    string private greeting;

    event GreetingChanged(string newGreeting);

    constructor() {
        greeting = "Hello Architect!";
    }

    function setGreeting(string memory newGreeting) public {
        greeting = newGreeting;
        emit GreetingChanged(newGreeting);
    }

    function getGreeting() public view returns (string memory) {
        return greeting;
    }
}
```
* >Simple contract. It stores a greeting, lets you update it, and emits an event when the greeting changes.
* >Save and exit: `Ctrl + X`, then `Y`, then `Enter`.

## Step 4: Write the Tests
* >Remove the default script folder and test file:
```
rm -rf script
rm test/Counter.t.sol
```
* >Create your test file:
```
touch test/HelloArchitect.t.sol
```
* >Open it:
```
nano test/HelloArchitect.t.sol
```
* >Paste this inside:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.30;

import "forge-std/Test.sol";
import "../src/HelloArchitect.sol";

contract HelloArchitectTest is Test {
    HelloArchitect helloArchitect;

    function setUp() public {
        helloArchitect = new HelloArchitect();
    }

    function testInitialGreeting() public view {
        string memory expected = "Hello Architect!";
        string memory actual = helloArchitect.getGreeting();
        assertEq(actual, expected);
    }

    function testSetGreeting() public {
        string memory newGreeting = "Welcome to Arc Chain!";
        helloArchitect.setGreeting(newGreeting);
        string memory actual = helloArchitect.getGreeting();
        assertEq(actual, newGreeting);
    }

    function testGreetingChangedEvent() public {
        string memory newGreeting = "Building on Arc!";
        vm.expectEmit(true, true, true, true);
        emit HelloArchitect.GreetingChanged(newGreeting);
        helloArchitect.setGreeting(newGreeting);
    }
}
```
* >Save and exit: `Ctrl + X`, then `Y`, then `Enter`.

## Step 5: Test and Compile
* >Run the tests:
```
forge test
```
* >You should see all three tests passing. If they do, compile:
```
forge build
```

## Step 6: Generate a Wallet
* >You need a wallet to deploy with. Generate one using Foundry's cast:
```
cast wallet new
```
* >This gives you an address and a private key. Save both somewhere secure. Do not share your private key with anyone.
* >Open the .env file we created earlier:
```
nano .env
```
* >Add your private key to the .env file:
```
PRIVATE_KEY="0x..."
```
* >Replace 0x... with your actual private key. Save and exit nano by pressing `Ctrl + X`, then `Y`, then `Enter`.
* >Reload the environment variables:
```
source .env
```
You'll run this command again later whenever you add new variables to the .env file.

## Step 7: Fund Your Wallet
Go to faucet.circle.com, select Arc Testnet, paste the wallet address you just generated, and request testnet USDC.

IMAGE👇

* >Remember, USDC is the gas token on Arc. This is what pays for your deployment and transactions. No ETH needed.

## Step 8: Deploy the Contract
Run this command to deploy:
```
forge create src/HelloArchitect.sol:HelloArchitect \
  --rpc-url $ARC_TESTNET_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
After a successful deployment, you'll see output with your deployer address, the deployed contract address, and a transaction hash. 
like this 👇

IMAGE

Copy the contract address from the "Deployed to:" line. Open your .env file:
```
nano .env
```
Add the contract address on a new line:
```
HELLOARCHITECT_ADDRESS="0x..."
```
* >Replace 0x... with your actual deployed contract address. Save and exit: `Ctrl + X`, then `Y`, then `Enter`.

Reload your variables again:
```
source .env
```
## Step 9: Interact With Your Contract
Call the getGreeting function on your deployed contract:
```
cast call $HELLOARCHITECT_ADDRESS "getGreeting()(string)" \
  --rpc-url $ARC_TESTNET_RPC_URL
```
* >You should see "Hello Architect!" returned.

## Step 10: Verify on the Explorer
Go to testnet.arcscan.app and paste the transaction hash from your deployment output. You'll see your contract deployment confirmed on the Arc Testnet explorer.

---
<div align="center">

## **Getting Started (Mac)**

</div>

## Open your built in Terminal app. That's all you need.

Install Foundry:
```
curl -L foundry.paradigm.xyz | bash
```
Reload your shell config:
```
source ~/.zshenv
```
Install the Foundry tools (forge, cast, anvil, chisel):
```
foundryup
```
You're good to go

# *What's Next* : 
From here, you can build further on your contract, add custom logic, or start exploring Arc’s native features like USDC for gas and fast finality. Since the mainnet is expected later in 2026, getting hands-on with the testnet now gives you a clear early advantage.

# Done✔️✔️

<pre>

Stay Connected

👉 Join for more updates and future releases: https://linktr.ee/EarlyCTs

If you run into any issues, You can also reach me directly via DM.

Thanks for checking out this.

</pre>

<div align="center">

#  *❤️‍🔥 Happy Node Running 👨🏻‍💻*

</div>

