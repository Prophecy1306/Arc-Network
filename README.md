<div align="left">

##   **Introduction📔**

Arc is a Layer 1 blockchain by Circle built for stablecoin-based finance, and it’s EVM-compatible, so if you’ve used Ethereum or similar chains it will feel familiar it’s currently live on public testnet with mainnet expected later in 2026 and one key thing to know is that gas fees are paid in USDC instead of ETH, meaning you won’t need test ETH since you’ll get testnet USDC directly from Circle’s faucet.

</div>
<div align="center">

#  👨🏻‍💻 **Deploy Your First Smart Contract on Arc Testnet** 👨🏻‍💻 

<img width="1536" height="620" alt="ChatGPT Image Mar 21, 2026, 06_28_42 PM" src="https://github.com/user-attachments/assets/aee0172d-af3a-42ec-8514-cba435b2f0ed" />

</div>

## **Getting Started (Windows)⚙️**

You'll need WSL (Windows Subsystem for Linux) to follow this guide. If you don't have it yet, open PowerShell as Administrator and run:
```
wsl --install
```
* >Restart your PC after installation, then open "Ubuntu" from your Start menu. That gives you a Linux terminal inside Windows.

### Install Foundry:
```
curl -L foundry.paradigm.xyz | bash
```
Reload your shell config:
```
source ~/.bashrc
```
Install the Foundry tools (forge, cast, anvil, chisel):
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

