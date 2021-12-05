# Blockchain Security

<aside>
🔐 Smart Contract Security

# Risk Assessment **and Philosophy**

- **Industry Outlook**
    - Cost of failure is very high
    - immutability makes mitigation and change very difficult
    - similar to hardware programming.
    - Lot of opportunities for a space in its infancy
    
    There is a rush to market within the current web3 landscape at the cost of proper execution. The high cost of failure and crunched delivery timelines combined with the a lack of general security expertise, creates a huge gap in the overall security posture in the web3 space.
    
    - **Blockchain Industry Elements**
        - Layer 1
            - Underlying blockchain protocol
            - Bitcoin Core, Geth
        - Layer 2
            - Overlaying network on top of layer 1 focused on scalability
            - Bitcoin lightning network
        - Smart Contracts
            - Software programs that are deployed on a blockchain that use code to execute terms of a contract / transaction.
            - Tokens, dApps, NFTs, etc.
                - All tied to a smart contract somewhere on the blockchain
        - Software Wallets
            - Custodial - online wallet
            - Non-custodial - in-control of your wallet
        - Hardware wallet
            - Physical devices for storing your private keys
        - Mining Software
            - Software / hardware used to mine tokens
        - Centralized Exchanges
            - Require KYC
            - Coinbase, Binance, etc
        - Decentralized Finance
            - Built via smart contracts w/ web3 front end for users to interact with
        - People
            - Social engineering, rug pulls, asset protection
- **Smart Contract Development Context**
    
    It is necessary to be aware of the attributes of smart contracts and how they contribute to vulnerabilities
    
    **Smart Contract Security Context**
    
    - All code is public
    - All transactions are public
    - All code is most likely immutable
        - Cannot fix bugs / exploits if discovered and transactions are irreversible
    
    **Awareness of Blockchain Properties**
    
    - Be very cautious of external contract calls that may execute malicious code
    - Public functions may be called maliciously
    - Private data in contracts is viewable by anyone.
        - just means that the contract can only be executed by the contract and not subcontracts
    - Miners can influence timestamps
    - Entropy / Randomness is trivial on blockchain and can be gamed
    
    **Fundamental Tradeoffs**
    
    - Rigid vs. Upgradeable
        - Flexibility by nature introduces complexity and more attack surfaces (such as a proxy contract)
    
    **Prepare for Failure**
    
    - Contract should have ability to be paused
    - Manage money at risk (rate limiting, max usage)
    - Bug fixes and upgrade path
    
    **Rollout Carefully**
    
    - Always better to catch bugs before a full production release
    - Test contracts thoroughly
    - Test and rollout in phases
    
    **Simplicity**
    
    - Complexity introduces more room for errors
    - Modularize code to keep key functions small
    - Use established libraries
    - Clarity > Performance
    - Not everything needs blockchain
- **Auditing Approach**
    
    ![Untitled](Blockchain%20Security%205d960a2fede640419e61d46e24894f77/Untitled.png)
    
- **SCSVC**
    
    A framework to threat model and analyze risk for smart contracts
    
    - **V1: Architecture, Design, and Threat Modeling**
        
        Analyze all possible threats before implementation of a smart contract. It is necessary to ensure that:
        
        - All related smart contracts are identified and used properly
        - Specific smart contract security assumptions are identified and used properly
        
        Requirement Examples
        
        - Verify mechanism to pause contract
        - Document fallback functions
        - Identify Solidity version
    - **V2: Access Control**
        
        Ensure that a verified smart contract satisfies the following:
        
        - Users and roles are well-defined
        - Access is granted only to privileged users and contracts
        
        Requirement Examples
        
        - Verify least privilege
        - Verify `tx.origin` is not used
        - Verify external calls only made if necessary
    - **V3: Blockchain Data**
        
        Smart contracts in public blockchains have no built-in mechanism to store data securely. Ensure that a contract satisfies the following:
        
        - Data stored is identified and protected
        - Secret data is not store in plaintext and is encrypted
        
        Requirement Examples
        
        - Verify any data stored is not considered sensitive
        
    - **V4: Communication**
        
        Communication between smart contracts and their libraries must satisfy the following high level requirements:
        
        - External calls from and to other contracts have considered abuse case and are authorized
        - Libraries in use are well-known and safe
        
        **Requirement Examples**
        
        - `delegatecall()` is not used with untrusted contracts
        - prevent reentrancy by avoiding usage of `call()` or `send()`
        - Verify third party contracts don't shadow special functions (revert, self-destruct)
    - **V5: Arithmetic**
        
        The smart contract must handle mathematical operations and any extreme variable values in a safe and predictable manner.
        
        **Requirement Examples**
        
        - Verify values are resistant to integer overflows / underflows
        - Verify that extreme values are considered and do not impact logic
        
    - **V6: Malicious Input Handling**
        
        The function parameters being passed must be handles in a safe and predictable manner
        
        **Requirement Examples**
        
        - Input must be validated
        - Length of smart contract address must be validated to prevent short address attack
        
    - **V7: Gas Usage and Limitations**
        
        The efficiency of gas usage must be taken into account for optimization and also to prevent possible exhaustion. A contract should satisfy the following:
        
        - Gas is optimized and unnecessary losses are prevented
        
        **Requirement Examples**
        
        - Verify contract does not iterate over unbound loops
        - Verify the `External` keyword is used for functions that can only be called externally to save gas
        - Verify that gas usage is anticipated and has clear limitations
        
    - **V8: Business Logic**
        
        Smart contracts can be vulnerable by their design and components used should not be considered safe without verification. Business assumptions should meet with the principle of minimum trust (don't use other network systems unless necessary). The following are high level requirements:
        
        - Business logic flow is sequential and in order
        - Business limits are specified
        - Cryptocurrency and token business logic flows have considered abuse cases
        
        **Requirement Examples**
        
        - Verify intended functionality
        - Verify contract logic does not hinge on balance of contract
        - Verify no process in the business logic can be skipped
        
    - **V9: Denial of Service**
        
        Ensure the contract logic prevents influencing the availability of the contract
        
        **Example Requirements**
        
        - Ensure self-destruct is used only if necessary
        - Business logic flow is not stopped when a participant is absent
    - **V10: Token**
        
        Ensure that the contract follows the relevant token security standards
        
        **Requirement Examples**
        
        - Verify token contract follows a tested token standard
        - Verify that contract does not allow to transfer tokens to zero address
    - **V11: Code Clarity**
        
        Ensure that the contract fills the following high-level requirements:
        
        - Code is clear and easy to read
        - There is no dead code
        - Variable names are in line with good practices
        - Functions utilized are secure
        
        **Requirement Examples**
        
        - Logic should be clear and modularized
        - Variable naming rules are consistent
        - Verify all variables are defined as storage or memory
        - Verify all stored variables are initialized
    - **V12: Test Coverage**
    - **V13: Known Attacks**
    - **V14: DeFi**
        
        This covers security requirements for constructions used within DeFi applications such as lending pools, flash loans, governance, etc.
        
        Ensure that the contract fills the following high-level requirements:
        
        - Assets controlled by DeFi are safe and cannot be withdrawn by an unauthorized person
        - The data sources (oracles) cannot be manipulated
- **Coinbase - Top Smart Contract Risks**
    
    The following are the most common types of risks associated with smart contracts surrounding ERC-20 implementations
    
    - **Operational Risks**
        
        Authorization features that are exploited when token network governance is insufficient or flawed
        
        **SCR-1: Super User Account or Privilege Management**
        
        The smart contract allows a privileged user that is able to perform changes to the functionality of an asset without restriction or checks
        
        **SCR-2: Blacklisting and Burning Functions**
        
        A privileged user may prohibit a specific address from performing key functionality
        
        **SCR-3: Contract Logic or Asset Configuration can be arbitrarily changed**
        
        The smart contract implements functions that allow the holder of a privileged role to unilaterally and arbitrarily alter the functionality of the asset.
        
        **SCR-4: Self-Destruct Functions**
        
        The smart contract has a function where a privileged role may remove the token contract from the blockchain and destroy all tokens created
        
        **SCR-5: Minting Functions**
        
        The smart contract has a function where a privileged role can increase the number of tokens in circulation or the balance of an arbitrary account.
        
    - **Implementation Risks**
        
        **SCR-6: Rolling Your Own Crypto and Unique Contract Logic**
        
        The smart contracts uses complex logic or non-standard libraries can introduce additional vulnerabilities
        
        **SCR-7: Unauthorized Transfers**
        
        The smart contract has functions that circumvent standard authorization patterns for sending tokens from an account
        
        **SCR-8: Incorrect Signature Implementation or Arithmetic**
        
        The smart contract operations lead to unintended states or results
        
    - **Design Risks**
        
        **SCR-9: Untrusted Control Flow**
        
        The smart contract triggers functions in external contracts that are not present within itself
        
        **SCR-10: Transaction Order Dependence**
        
        The smart contract allows asynchronous transaction processing that can be exploited through front-running and mempool reordering
        
- **Threat Modeling a General Blockchain Solution**
    
    [Instant Threat Modeling - #17 Hacking Blockchain Security](https://www.youtube.com/watch?v=IwR4PAmRhhg)
    
    **Potential Attack Vectors**
    
    ![Untitled](Blockchain%20Security%205d960a2fede640419e61d46e24894f77/Untitled%201.png)
    
    **Remediations**
    
    ![Untitled](Blockchain%20Security%205d960a2fede640419e61d46e24894f77/Untitled%202.png)
    

# Solidity and Ethereum

- **Blockchain Basics**
    
    ### **What is a Blockchain**
    
    - A chain of blocks containing information on a distributed ledger
    - Immutable and decentralized
    
    ![Untitled](Blockchain%20Security%205d960a2fede640419e61d46e24894f77/Untitled%203.png)
    
    - contains data (transaction data)
    - Current Hash (like a fingerprint)
    - Hash of previous block
    
    ### **Proof of Work vs Proof of Stake**
    
    ![Untitled](Blockchain%20Security%205d960a2fede640419e61d46e24894f77/Untitled%204.png)
    
    ### ERC20 Tokens
    
    ERC20 is the standard by which tokens are created on the ethereum network. Reduces need for custom code and allows exchanges and wallets to easily implement new tokens.
    
    - Mandatory Functions
        - `totalSupply` - defines total supply of token
        - `balanceOf` - returns how many tokens an address has
        - `transfer` - takes tokens from total supply and gives to user
        - `transferFrom` - used to transfer between any two users with tokens
        - `approve` - verifies your contract can give an amount to user taking into account the total supply
        - `allowance` - verifies if user has enough to send certain amount of tokens to another user
    - ERC20 operates as an interface
        - implementation forces you to have the 6 standards
- **Ethereum Network Overview**
    
    ![Untitled](Blockchain%20Security%205d960a2fede640419e61d46e24894f77/Untitled%205.png)
    
    **What is Ethereum?**
    
    Network
    
    - Ethereum is a P2P network of Nodes talking to one another
    - To use the Ethereum network you need access via:
        - Running your own node
        - Connecting to a node via a URL which will act on your behalf
    
    Database
    
    - DB of transactions storing entire history of transactions on the blockchain
    - Transactions are grouped into blocks
    
    Smart Contracts
    
    - Code that is written to the blockchain that relies on the database to perform actions
    
    Proof of Work
    
    - Miners compete to complete cryptographic puzzles in order to "mine" a block.
        - This occurs whenever data / a transaction is written to the blockchain
    - The transaction cost is known as **gas** which is paid by the individual initiating the transaction
    - Process Summary
        1. Sender submits a transaction to the network with a transaction fee
        2. Network creates a cryptographic puzzle for the miner to solve
        3. Miners brute-force to identify the solution to the puzzle
        4. Winning miner gets the fee and the transaction is added to the network
        5. Recipient received funds
    
    Security of Transactions
    
    - It is nearly impossible to tamper with data on the blockchain
        - A change requires ALL transactions that have occurred in the past
        - **51% Attack** - Require 51% of all computational power to tamper with the blockchain
    
    Smart Contracts
    
    - Code that runs on the blockchain
    - Building blocks for decentralized applications
    - Operate like vending machines - you know exactly what you're getting from it
        - Intended to function the same way every time
    
    Proof of Stake
    
    - Consensus algorithm where Ethereum is locked up (staked) to become validators
    - Validators include new transactions on the network
- **Secure Development Recommendations**
    
    It is recommended to exercise caution and follow best practices when developing smart contracts
    
    **External Calls**
    
    - Calls to untrusted contracts can introduce risks such as malicious code execution or any contract the calling contracts depends on
    - Use only when absolutely needed
    - Mark information about untrusted contracts in code
    
    ```jsx
    // bad example
    Bank.withdraw(100); // Unclear whether trusted or untrusted
    
    function makeWithdrawal(uint amount) { // Isn't clear that this function is potentially unsafe
        Bank.withdraw(amount);
    }
    
    // good example
    UntrustedBank.withdraw(100); // untrusted external call
    TrustedBank.withdraw(100); // external but trusted bank contract maintained by XYZ Corp
    
    function makeUntrustedWithdrawal(uint amount) {
        UntrustedBank.withdraw(amount);
    }
    ```
    
    - Avoid state changes after external calls
        - Assume malicious code could be executed
    - Handle errors in external calls
        - low level calls like `**delegatecall()`** will not throw an exception but will return `false` if an exception occurs
        - If usage of low level calls occur, logic to check if calls fails by looking at return value is needed
    - Never make `**delegatecalls()**` to untrusted code
        - functions will run in caller contract's context
    - Avoid Usage of `transfer()` or `send()`
        - Both forward exactly 2300 gas to the recipient
        - higher potential for reentrancy
        - better to use `**call()**`
    
    **Solidity Specific**
    
    - Use modifiers only for checks and to replace duplicate checks
        - an external call in a modifier can lead to reentrancy
    - Keep fallback functions simple
    - Explicitly mark visibility in functions and state variables
        - `External` functions are part of the contract interface. An external function `f` cannot be called internally (i.e. `f()` does not work, but `this.f()` works). External functions are sometimes more efficient when they receive large arrays of data.
        - `Public` functions are part of the contract interface and can be either called internally or via messages. For public state variables, an automatic getter function (see below) is generated.
        - `Internal` functions and state variables can only be accessed internally, without using `this`.
        - `Private` functions and state variables are only visible for the contract they are defined in and not in derived contracts. **Note**: Everything that is inside a contract is visible to all observers external to the blockchain, even `Private` variables.
    - Avoid usage of `tx.origin`
        - it is a global variable that returns the address of the account that sent the transaction
        - Using `tx.origin` for authorization can make a contract vulnerable if an authorized account calls into a malicious contract
    - Timestamp Manipulation by miners
- **Solidity Concepts**
    
    Solidity is an object-oriented, high-level language for implementing smart contracts on Ethereum
    
    **Immutability**
    
    - The code you deploy is permanent and acts as law
    - Whenever you call a function that's the function that's called
    - Due to immutability, better to have functions that allow you to update key areas of a DApp
    
    **Ownable Contracts**
    
    - Contracts that have an owner with special privileges
    - Typically ownable contracts perform the following:
        1. When a contract is deployed, the constructor sets the *owner* to *`msg.sender`* (person deploying the contract)
        2. Adds an `onlyOwner` modifier, enables restrictions to certain functions only to *owner*
        3. Allows transfer of contract to new *owner*
    
    **Constructors**
    
    - An optional special function that has the same name as the contract
    - Executed only one time once the contract is first created
    
    **Function Modifiers**
    
    - Half-functions used to modify other functions
    - Usually used to check some requirement prior to execution
    - Looks just like a function but uses the keyword `modifier` instead of `function`
        - Cannot be called like a function, `modifier` is instead attached to the end of a function definition
    - Modifier Examples
        
        ```jsx
        modifier **onlyOwner**() {
        require(isOwner());
        _;
        }
        ```
        
        Modifier Execution
        
        ```jsx
        function renounceOwnership() public **onlyOwner** {
            emit OwnershipTransferred(_owner, address(0));
            _owner = address(0);
          }
        ```
        
- **Assessing ERC20 Token Security**
    
    ERC-20 is the Ethereum Smart Contract Standard. Although the standard is simple, the diversity in its implementation is vast.
    
    **4 Core Qualities to Secure Smart Contracts**
    
    1. Verified Source Code Usage
    2. Industry Standard Library Usage
    3. Limited Scope for Privileged Users
    4. Simple and Modular Design
    
    **Verified Source Code Usage (#1)**
    
    Without access to source code an auditor cannot analyze token behavior.
    
    In order to verify a token's code, one must do the following:
    
    - Upload the source code for all smart contracts to Etherscan or another reliable platform
    - Add the code to a public repository such as GitHub
    - Upgradeable tokens should use distinct releases to communicate the state of the token at each upgrade
    
    **Industry Standard Library Use (#2)**
    
    It is better to avoid writing as much smart contract code from scratch as possible. A single mistake can compromise the integrity of an asset. Popular libraries are well-vetted and scrutinized.
    
    Instead of building from scratch one can:
    
    - Use popular libraries such as OpenZeppelin (vast repository of smart contracts)
    - Use EIPs as guidance when implementing special or unique features
    
    **Limited Scope for Privileged Roles (#3)**
    
    Tokens often have superuser roles such as "admin" or "owner". These roles hold significant power to call functions to perform action such as:
    
    - Modifying transactions and balances
    - Changing contract logic
    - Modify user funds
    
    Best practices when limiting privileged roles include:
    
    - Not allowing any role to freeze, burn, or otherwise modify user funds without permission
    - Upgrade pattern / flow should not be determinant on a single privileged role to perform upgrades unilaterally. There should be some degree of consensus / agreement prior to an upgrade occurring
        - If the above is not feasible, robust quorum-based key management practices must be followed, especially in regards to upgrading features where user balance is affected.
        - Keys would be held by a custodian until quorum is met to take action
    
    **Simple Modular Design (#4)**
    
    Simple code utilizing only components that are necessary and modular in terms of being able to separate logic from responsibilities between contracts.
    
    Higher Contract Complexity —> Higher Risk of Vulnerability
    
    To reduce token complexity one can:
    
    - Keep token-related functions minimal by separating the token contract from the rest of the protocol
    - Reduce or eliminate external token dependencies
    - Use fewer contracts when implementing a token

# Solidity Smart Contract Vulnerabilities

### **Real Life Smart Contract Hacks**

- **The DAO Hack**
    
    An attacker identified a reentrancy vulnerability in which they were able to recursively call the split function and retrieved their funds multiple times before getting to the step where the code would check the balance
    
- **Parity Hack**
    
    An attacker used the `initWallet` function on the Parity library contract and passed his own address as the owner in order to become the single owner of the contract.
    
    This was possible because Parity left the library contract uninitialized and open to attack.
    
    After gaining ownership of this contract, the attacker used the `kill` function to destroy the library contract and resulted in a loss of $150 million dollars.
    
    **Vulnerable Code**
    
    ![Untitled](Blockchain%20Security%205d960a2fede640419e61d46e24894f77/Untitled%206.png)
    
- **Polygon Hack**
    
    The Poly network was a cross-chain collection of smart contracts allowing for interoperability between chains.
    
    *Keepers* were trusted entities for cross-chain transactions. These *Keepers* would receive a transaction from users and sign the block and the contract will execute as the *EthCrossChainManager* role
    
    This role had the ability to change *Keepers* via the *EthCrossChainData* contract and allowed an attacker to assume the role of a *Keeper*
    
    The attacker signed fake transactions, resulting in large financial loss
    
- **Cream Finance Hack**
    
    An attacker abused flash loans in order to steal a large amount of funds.
    
    Flash loans are the ability for a contract to provide funds to a user in a single transaction but the funds must be returned in the exact same transaction. If the funds will not be returned on a programatic level, the transaction will be reverted.
    
    A function in the contract allowed for reentrancy and nest a second borrow function and borrow much more Ethereum than what was needed to repay flash loan.
    

### **Smart Contract Specific Vulnerabilities**

- **Reentrancy**
    
    This vulnerability is introduced when calling external contracts as it can lead to a change in control flow and changes to the data of the calling function that were not intended
    
    **Single Function Reentrancy**
    
    This vulnerability involves the repeated calling of a function as a result of the fallback function from an attacker contract. Additionally, if the vulnerable function does the accounting of the transaction after the invoked function to transfer funds, an attacker could drain the wallet:
    
    Example
    
    ![Untitled](Blockchain%20Security%205d960a2fede640419e61d46e24894f77/Untitled%207.png)
    
    ```jsx
    function withdrawBalance() public {
        uint amountToWithdraw = userBalances[msg.sender];
        (bool success, ) = msg.sender.call.value(amountToWithdraw)(""); // At this point, the caller's code is executed, and can call withdrawBalance again
        require(success);
        userBalances[msg.sender] = 0;
    }
    ```
    
    Additionally any function that called the vulnerable `withdrawBalance()` would also in turn be vulnerable
    
    Recommendation
    
    It is recommended to not call an external function until all the internal work has been completed such as accounting
    
    ```jsx
    function withdrawBalance() public {
        uint amountToWithdraw = userBalances[msg.sender];
        userBalances[msg.sender] = 0;
        (bool success, ) = msg.sender.call.value(amountToWithdraw)(""); // The user's balance is already 0, so future invocations won't withdraw anything
        require(success);
    }
    ```
    
    **Cross-function Reentrancy**
    
    This attack can also be performed using two different functions that share the same state
    
    An attacker contract can call another function within the same contract such as in the vulnerable example below:
    
    ```jsx
    function transfer(address to, uint amount) {
        if (userBalances[msg.sender] >= amount) {
           userBalances[to] += amount;
           userBalances[msg.sender] -= amount;
        }
    }
    
    function withdrawBalance() public {
        uint amountToWithdraw = userBalances[msg.sender];
        (bool success, ) = msg.sender.call.value(amountToWithdraw)(""); // At this point, the caller's code is executed, and can call transfer()
        require(success);
        userBalances[msg.sender] = 0;
    }
    ```
    
    Remediation
    
    It is recommended to finish all internal operations (state changes) first, and only then call an external function.
    
- **Transaction Order Dependence (Race Conditions / Front Running)**
    
    This vulnerability exists due to the ability of a miner knowing which transactions will occur before being finalized. As a result, there exists a race condition whenever code depends on the order of the transactions submitted.
    
    **Example 1**
    
    *A smart contract exists that gives a reward of 10 $MATH tokens for solving a math problem:*
    
    - John submits an answer and pays 50 GWEI to make the transaction
    - Mark runs his own node and can view the transaction with the answer before it is mined
    - Mark submits the copied answer in his own transaction with a gas price of 100 GWEI
    
    **Result**
    
    - Mark wins and receives the $10 Math tokens
    - John loses and receives nothing
    - Why?
        - The higher gas price is preferred when a race condition exists
    
    **Example 2**
    
    This vulnerability exists most on the ERC20 token standard within the `approve()` function, which allows an address to approve another address to spend tokens on their behalf. 
    
    *John has approved Mark to spend 100 $MATH tokens on his behalf:*
    
    - John changes the approved amount Mark can spend to 40 $MATH tokens
    - Mark runs his own node and sees this pending transaction to change his approval amount
    - Mark front runs John's approval change transaction with a transaction of his own with higher gas allowing him to still request 100 $MATH tokens.
    - John's transaction for Mark's approval to change to $40 MATH tokens goes through
    - Mark is able to submit another request to withdraw the $40 MATH tokens now
    
    **Result**
    
    - Mark now has $140 MATH tokens from this vulnerability
    - Why?
        - Mark front-ran John's approval request with higher gas before it went through with his own transfer request of the previously approved amount
        
- **`Tx.Origin` Usage**
    
    `tx.origin` is a global variable in Solidity which returns the address of the account that sent the transaction. Using the variable for authorization could make a contract vulnerable if an authorized account calls into a malicious contract. 
    
    A call could be made to the vulnerable contract that passes the authorization check since `tx.origin` returns the original sender of the transaction which in this case is the authorized account.
    
    **Example**
    
    ![Untitled](Blockchain%20Security%205d960a2fede640419e61d46e24894f77/Untitled%208.png)
    
- **Timestamp Dependency**
    
    The timestamp of the block can be manipulated by the miner, and all direct and indirect uses of the timestamp should be considered.
    
    Contracts using `block.timestamp` or `block.number` can be altered by miners within a range of 14 seconds and therefore should not be utilized in any time-sensitive logic.
    
- **Integer Overflow and Underflows**
    
    Integer overflow and underflows often occur when user supplied data controls the value of an unsigned integer. The user supplied data either adds to or subtracts beyond the limits the variable type can hold. 
    
    This can be performed to bypass certain checks in a smart contract.
    
    **Example**
    
    This operates similar to how an odometer would work for an overflow
    
    - Once an odometer hits its limit (999999) it goes back to 0
    
    Overflows and underflows can occur by performing arithmetic operations that exceed max or minimum integer values
    
    - Unsigned Integer Range
        - 2^256 -1
    - Signed Integer Range
        - 2^255 to 2^256-1
    - Uint Overflow Example
        - 1 +(2^256-1) = **0**
        - Need libraries / security checks to prevent this
    
    **Example Exploit**
    
    - Say you have a timelock on a function
    - If you can overflow / underflow that value for the timelock then you could roll it back it down to the lowest possible time
    

### Solidity Problematic Language Design

- **Access Specifiers**
    
    `External` functions are part of the contract interface. An external function `f` cannot be called internally (i.e. `f()` does not work, but `this.f()` works). External functions are sometimes more efficient when they receive large arrays of data.
    
    `Public` functions are part of the contract interface and can be either called internally or via messages. For public state variables, an automatic getter function (see below) is generated.
    
    `Internal` functions and state variables can only be accessed internally, without using `this`.
    
    `Private` functions and state variables are only visible for the contract they are defined in and not in derived contracts. 
    
    - **Note**: Everything that is inside a contract is visible to all observers external to the blockchain, even `Private` variables.
- **Fallback Functions**
    
    Fallback functions are called by default whenever no method matches with the external request. 
    
    They are also called whenever money is being sent over and require the `payable` modifier in order to receive money.
    
- **Unchecked Low Level Calls**
    
    One of the more in-depth features of Solidity are the low-level functions such as `call()`, `callcode()`, `delegatecall()` and `send()`. 
    
    Errors will not surface immediately and will not lead to the total reversal of the current execution. Instead, they will return a boolean value set to false and the code will continue to run. 
    
    If the return value of such low-level calls is not checked, it could lead to “fail open” situations and other unwanted outcomes.
    
- **Dynamic Libraries**
    
    The usage of third party libraries often puts additional risk if they are not properly vetted. The usage of a `delegatecall()` can result in running malicious code in the context of the calling contract rather than the contract being called.
    
</aside>