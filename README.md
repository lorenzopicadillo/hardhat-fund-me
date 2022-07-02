L7. Hardhat - Fund Me
-
- $ yarn add --dev hardhat
- $ yarn hardhat
    // $ create advanced sample project
- $ yarn solhint contracts/*.sol
    // solhint goes over solidity contracts to check for errors
- $ yarn add --dev @chainlink/contracts
- copypasta FundMe & PriceConverter
- $ yarn hardhat compile
- HH Deploy Package 
    // $ yarn add --dev hardhat-deploy
    // $ mkdir deploy
    // $ yarn add --dev @nomiclabs/hardhat-ethers@npm:hardhat-deploy-ethers ethers
    // numbered scripts for each objective eg. 01-deploy-fund-me.js
    // for getNamedAccounts we'll have a deployer, configed @ hardhat.config.js
- Mocking & helper config
    // Programatic changes flow: (v ~10:25)
        // using PriceConverter as a library on top of uint256 type
        // calling getConversionRate with msg.value 
    // refresher
        // parametrising priceFeedAddress & passing it as a constructor -> saving it as a global variable as a AggregatorV3Interface type
        // passing it to a getConversionRate() -> passes it to getPrice() -> calls latestRoundData()
        // we could've got rid of getPrice() by pasting its code into getConversionRate()
    ...
    // $ yarn hardhat deploy --tags mocks
        // only runs mocks
    // We set it up so that programatically we can work on multiple local and external networks
    // $ yarn hardhat deploy
    // $ yarn hardhat node
        // Will launch a local blockchain node w addresses, pkeys & our contracts deployed 
- Utils Folder 
// new file verify.js in utils folder
    //
- Testnet Demo
    // Added rinkeby network process.env const to hardhat.config.js, 
    // $ yarn hardhat deploy --network rinkeby
    // if we wanted to add more network it would suffice to add it on hardhat.config and helper HH config
- Solidity Style guide
    // Contract Layout
        // 1 Pragma statements
        // 2 Import statements
        // 3 Interfaces
        // 4 Libraries
        // 5 Contracts 
    // Errors should be designed and coded to identify the contract that errored
    // NatSpec Format. Ethereum Natural Language Specific Format. Context in '///  ///' or '/** */' comments to explain in plain english
    // Inside each contract, interface or library
        // Type declaration
        // State variable
        // Events 
        // Modifiers
        // Functions Order:
            // constructor
            // receive
            // fallback
            // external
            // public
            // internal
            // private
            // view/pure
- Testing
- Breakpoints & debugging (v ~11:30)
    // Adding breakpoints
    // JS debud terminal for access to variables and data 
- console.log & debugging (v ~11:36)
    // $ import "hardhat/console.sol";
    // now we can console.log withing solidity code
- Testing Fund Me II, testing for multiple funders
    // We need to .connect() fundMe to each funding account w for loop, because originally we connected fundMe w deployer
- Storage in Solidity. Different elements require diff gas to run
    // const only point to the value, aren't stored
    // variable within functions only exist there, they're not stored
- Gas optimization using storage knowledge (v ~11:54)
    // bytecode
    // opcode
        // by gas https://github.com/crytic/evm-opcodes
        // SSTORE & SLOAD are high gas consumption, specially store > 20000
        // storage notation is 's_variableName'
        // immutabel notation is 'i_variableName'
    // cheaperWithdraw()
        // huge saving when saving and reading from memory instead of storage
        // mappings cannot be in memory, must be in storage
- Solidity styling guide.
    // Right order of elements, view/pure at the end with getFunctions() to improve readability of code wo s_variables or i_variables 
- Storage Review (v 12:10)
    // constants, immutable variables & memory don't go to storage
    //




# Advanced Sample Hardhat Project

This project demonstrates an advanced Hardhat use case, integrating other tools commonly used alongside Hardhat in the ecosystem.

The project comes with a sample contract, a test for that contract, a sample script that deploys that contract, and an example of a task implementation, which simply lists the available accounts. It also comes with a variety of other tools, preconfigured to work with the project code.

Try running some of the following tasks:

```shell
npx hardhat accounts
npx hardhat compile
npx hardhat clean
npx hardhat test
npx hardhat node
npx hardhat help
REPORT_GAS=true npx hardhat test
npx hardhat coverage
npx hardhat run scripts/deploy.js
node scripts/deploy.js
npx eslint '**/*.js'
npx eslint '**/*.js' --fix
npx prettier '**/*.{json,sol,md}' --check
npx prettier '**/*.{json,sol,md}' --write
npx solhint 'contracts/**/*.sol'
npx solhint 'contracts/**/*.sol' --fix
```

# Etherscan verification

To try out Etherscan verification, you first need to deploy a contract to an Ethereum network that's supported by Etherscan, such as Ropsten.

In this project, copy the .env.example file to a file named .env, and then edit it to fill in the details. Enter your Etherscan API key, your Ropsten node URL (eg from Alchemy), and the private key of the account which will send the deployment transaction. With a valid .env file in place, first deploy your contract:

```shell
hardhat run --network ropsten scripts/deploy.js
```

Then, copy the deployment address and paste it in to replace `DEPLOYED_CONTRACT_ADDRESS` in this command:

```shell
npx hardhat verify --network ropsten DEPLOYED_CONTRACT_ADDRESS "Hello, Hardhat!"
```
