
# Solidity Notes

2018-02-02

## Basic Contract Structure

```solidity
pragma solidity ^0.4.17;

contract myContract {
    uint256 myVariable; 
    address owner;
    
    modifier onlyowner() {
        require(owner == msg.sender);
        _; 
    }
    // must be payable if you want to send along any ether
    function myContract ()  payable public {
        myVariable = 5;
        owner = msg.sender;
    }
    
    function setMyVariable(uint myNewVariable) public onlyowner{
        // old school if then 
        myVariable = myNewVariable;
    }
    
    function getMyVariable() constant public returns (uint)  {
        return myVariable;
    }    
    
    function getMyOwner() constant public returns(address) {
        return owner;
    }
    
    function getMyContractBalance() constant public returns(uint) {
        return this.balance;
    }
    function () payable public  {
        // i don't know why i have this here
    }
    function kill() public {
        require (msg.sender == owner);
        selfdestruct(owner); //used to be suicide()
    }
    
}
```


## Functions

### Visibility modifiers
* ```private``` only callable by other functions inside the contract
* ```internal``` like private but are inherited by child contracts
* ```external``` are only called from outside the contract
* ```public``` are visible and callable from inside the contract as well as 
from other contracts
### State modifiers
* ```view``` does not change any information
* ```pure``` doesn't change anything nor does it even touch the blockchain 

## Data Types

- ```uint``` or ```uint256``` is a 256 bit wide unisigned integer
- ```address``` is an ethereum address
### Mappings
* like an associative array 
```solidity
// For a financial app, storing a uint that holds the user's account balance:
mapping (address => uint) public accountBalance;
// Or could be used to store / lookup usernames based on userId
mapping (uint => string) userIdToName;
```
* ```msg.sender``` is used to identify the account that called the contract
```
mapping (address => uint) favoriteNumber;

function setMyNumber(uint _myNumber) public {
  // Update our `favoriteNumber` mapping to store `_myNumber` under `msg.sender`
  favoriteNumber[msg.sender] = _myNumber;
  // ^ The syntax for storing data in a mapping is just like with arrays
}

function whatIsMyNumber() public view returns (uint) {
  // Retrieve the value stored in the sender's address
  // Will be `0` if the sender hasn't called `setMyNumber` yet
  return favoriteNumber[msg.sender];
}
```

## Interfaces

* Look a bit like a contract
* Allow us to talk to functions in other contracts
* Contain templates of the only the functions you care about.
* Functions have no body - semicolon instead of body tells the compiler this is an 
interface 

```
contract NumberInterface {
  function getNum(address _myAddress) public view returns (uint);
}

contract MyContract {
  address NumberInterfaceAddress = 0xab38... 
  // ^ The address of the FavoriteNumber contract on Ethereum
  NumberInterface numberContract = NumberInterface(NumberInterfaceAddress)
  // Now `numberContract` is pointing to the other contract

  function someFunction() public {
    // Now we can call `getNum` from that contract:
    uint num = numberContract.getNum(msg.sender);
    // ...and do something with `num` here
  }
}


```

