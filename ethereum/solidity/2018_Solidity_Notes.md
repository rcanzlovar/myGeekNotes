
# Solidity Notes

2018-02-02

## Basic Contract Structure

```solidity
pragma solidity ^0.4.17;

contract Adoption {
    // constructor - this code is run when you load this contract and 
    //sets up the initial variables etc. 
    function Adoption(uint petId) public returns (uint) {

    address[16] public adopters;
    
    // Retrieving the adopters
    function getAdopters() public view returns (address[16]) {
      return adopters;
    }

}
```


## Functions

* Visibility modifiers
- ```private``` only callable by other functions inside the contract
- ```internal``` like private but are inherited by child contracts
- ```external``` are only called from outside the contrac
- ```public``` are visible and callable from inside the contract as well as 
from other contracts
* State modifiers
- ```view``` does not change any information
- ```pure``` doesn't change anything nor does it even touch the blockchain 


## Mappings
- like an associative array 
```
// For a financial app, storing a uint that holds the user's account balance:
mapping (address => uint) public accountBalance;
// Or could be used to store / lookup usernames based on userId
mapping (uint => string) userIdToName;
```
- msg.sender is used to identify the account that called the contract
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

- Look a bit like a contract
- Allow us to talk to functions in other contracts
- Contain templates of the only the functions you care about.
- Functions have no body - semicolon instead of body tells the compiler this is an 
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


## Basics

- ```Update-Help``` (run as admin): Download newest Help files from Internet
- ```Get-Alias –Definition target```: show all aliases that use or point to target
- To get a single property value: ```(Get-Process -PID $PID).Name```
- To search for command: ```Get-Command -Noun process```

### Misc

- to create a string containing a line of X's: ```$line = "x"*77```
- to change the Powershell window title: ```$host.UI.RawUI.WindowTitle = $host.UI.RawUI.WindowTitle ="PS Window @ " + (get-date -Format g)```

## History

- h -> Get-History
- r -> Invoke-History 
- ```Get-History | ? -Property Id -gt 190```
- ```Get-History | ? CommandLine -like "dir*"```
- ```h |? CommandLine -like dir*```
- to run the fifth command from history: ```r 5```

See also: https://blogs.technet.microsoft.com/heyscriptingguy/2011/01/18/use-powershell-history-to-speed-repetitive-commands/

## Get-Member

- Get-Member: Gets the type, properties and methods of an object.
- gm: a built-in alias to Get-Member
- object.GetType() will return this Powershell’s object type and base type

## PowerShell Objects

- Page 69-70:
- ```$obj = [PSCustomObject]@{ 'Some Name' = "My Value" }```


- Page 74:

```powershell
$obj = New-Object Object
$obj | Add-Member -Name Age -Value 45 -MemberType NoteProperty
```

- *Note:* a NoteProperty is: A property defined by a Name-Value pair

See also: https://docs.microsoft.com/en-us/dotnet/api/system.management.automation.psmembertypes

## ForEach-Object vs ForEach

- alias: % -> ForEach-Object
- When you are piping input into **ForEach**, it is the alias for **ForEach-Object**. But when you place **ForEach** at the beginning of the line, it is a Windows PowerShell statement.
