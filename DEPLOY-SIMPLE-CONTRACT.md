## DEPLOY A SIMPLE CONTRACT USING HARDHAT

- Open Github Codespace/ Gitpod/ Linux based Terminal
- Create a folder
```bash
mkdir Swiss
```
```bash
cd Swiss
```
```bash
sudo apt-get remove -y nodejs
```
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash && export NVM_DIR="/usr/local/share/nvm"; [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"; [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"; source ~/.bashrc; nvm install --lts; nvm use --lts
```
```bash
npm install --save-dev hardhat
```
```bash
npm install --save-dev @nomicfoundation/hardhat-toolbox
```
```bash
npx hardhat
```
```bash
rm hardhat.config.js
```
```bash
nano hardhat.config.js
```
- Now paste these things in `hardhat.config.js` file, also make sure to input your private key
```bash
require("@nomicfoundation/hardhat-toolbox");

module.exports = {
  solidity: "0.8.19",
  networks: {
    swisstronik: {
      url: "https://json-rpc.testnet.swisstronik.com/",
      accounts: ["0xd5..."], //Your private key starting with "0x"
    },
  },
};
```
```bash
cd contracts
```
```bash
rm Lock.sol
```
```bash
nano Lock.sol
```
- Now paste these codes in the above file (`Lock.sol`)
```bash
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.19;

//This contract is only intended for testing purposes

contract Swisstronik {
    string private message;

    /**
     * @dev Constructor is used to set the initial message for the contract
     * @param _message the message to associate with the message variable.
     */
    constructor(string memory _message) payable{
        message = _message;
    }

    /**
     * @dev setMessage() updates the stored message in the contract
     * @param _message the new message to replace the existing one
     */
    function setMessage(string memory _message) public {
        message = _message;
    }

    /**
     * @dev getMessage() retrieves the currently stored message in the contract
     * @return The message associated with the contract
     */
    function getMessage() public view returns(string memory){
        return message;
    }
}
```
- Now use the below command to compile ur contract file (`Lock.sol`)
```bash
npx hardhat compile
```
```bash
cd ..
```
```bash
mkdir scripts && cd scripts
```
```bash
nano deploy.js
```
- Paste these codes in `deploy.js` file
```bash
const hre = require("hardhat");

async function main() {
  /**
   * @dev make sure the first argument has the same name as your contract in the Hello_swtr.sol file
   * @dev the second argument must be the message we want to set in the contract during the deployment process
   */
  const contract = await hre.ethers.deployContract("Swisstronik", ["Hello Swisstronik!!"]);

  await contract.waitForDeployment();

  console.log(`Swisstronik contract deployed to ${contract.target}`);
}

//DEFAULT BY HARDHAT:
// We recommend this pattern to be able to use async/await everywhere
// and properly handle errors.
main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```
```bash
cd
```
```bash
cd Swiss
```
```bash
npx hardhat run scripts/deploy.js --network swisstronik
```
- After successful deployment, you should receive the following message in your terminal: `Swisstronik contract deployed to 0x...`
