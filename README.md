* Blockchain lets you implement a business process when you need to trust data and participants without using a central database.
* blockchain relies on cryptography so its hard to change history of data
* coins are physcal pieces of ownership, you give the coin to a friend, it is now theirs
* in blockchain, say you transfer ownership that fact beomes a node that updates all ledgers
  * facts are validated by consensus of the ledger, if the coin is not yours it cant also be someone elses
  * each block in the block- chain ends up being turned into a sha-256 hash
  * blockchain uses hashes to determine if there has been any change made
* so A __decentralized application (DApp)__ is an application on a distributed computing system
  * Ethereum DApps are called smart contracts.
  * __Ethereum DApps__ contains logic parts of a transaction
  * you use Solidity to program this logic
  * to use an __Ethereum DApp__ you make an instance so  __Ethereum DApp__ is like a class, with metadata about the item and fns to say transfer ownership
  * __Ethereum DApp__ is immutable


## Blockchain Types
* public or private to people

### Public
* no trust

### Private
* partial trust, transactions are validation against a consortium

### Blockchain protocols
* bitcoin is one of the several blockchain protocols
* use  Ethereum and Hyperledger Fabric if you have your own solition

## When to use blockchain
* how many participants, do we want to avoid central trust, do we rely on synchornize
* how much business logic is needed when doing a transction, if its high dont use blockchain?
* when the business logic is very static, changes requires new etherum DApps which are expensinve

## Blockchain options on Azure
* __templates__ are like your own custom blockchain
* __Azure Blockchain Workbench__ is for development testing and getting used to blockchain development


# Learn How To Use Solidity
* solidity is a language for writing smart contracts logistics environment if you will

## What is Solidity
* __Solidity__ is an object-oriented language for writing smart contracts.
* Solidity smart contracts will be deployed to and run in a virtual environment.
* Ethereum is one of the most popular blockchain platforms, 

## Understand the language basics
* to get around in solidity
  * Pragma directives
  * State variables
  * Functions
  * Events

### Pragma directives

__Pragma__ is the keyword that you use to ask the compiler to check whether its version of Solidity matches the one required.
* make sure you are using the latest version of [solidity](https://docs.soliditylang.org/en/v0.8.15/)

![1659450050635](image/README/1659450050635.png)

### State variables
State variables are key to any Solidity source file. State variable values are stored in contract storage permanently.
```sol
pragma solidity >0.7.0 <0.8.0;

contract Marketplace {
    uint price; // State variable
```
* Contract source files always start with the definition contract ContractName.

state variable visibility specifiers
* public: part of the contract interface and can be accessed from other contracts.
* internal: only accessed internally from the current contract.
* private: only visible for the contract it's defined in.

### Functions
* just the same in other languages
* visibility specifiers: public, private, internal, and external.
* A function can be called internally or externally from another contract.
```sol
pragma solidity >0.7.0 <0.8.0;

contract Marketplace {
    function buy() public {
        // ...
    }
}
```

### Function modifiers
* Function modifiers can be used to change the behavior of functions. They work by checking a condition before the function executes.
```sol
pragma solidity >0.7.0 <0.8.0;

contract Marketplace {
    address public seller;

    modifier onlySeller() {
        require(
            msg.sender == seller,
            "Only seller can put an item up for sale."
        );
        _; // where the fn gets inserted
    }

    function listItem() public view onlySeller {
        // ...
    }
}
```


### Events
* they end up in a transaction list for the ledger cant be modified or viewed in the contract
```sol
pragma solidity >0.7.0 <0.8.0;

contract Marketplace {
    event PurchasedItem(address buyer, uint price);

    function buy() public {
        // ...
        emit PurchasedItem(msg.sender, msg.value);
    }
}
```


## Explore value types


### Integers
* whole nums, signed and unsigned,storage size from 8 bits to 256 bits.

* __Signed__: Includes negative and positive numbers. Can be represented as int.
* __Unsigned__: Includes positive numbers only. Can be represented as uint.
```sol
int32 price = 25; // signed 32 bit integer
uint256 balance = 1000; // unsigned 256 bit integer

balance - price; // 975
2 * price; // 50
price % 2; // 1
```

### Booleans
```sol
bool forSale; //true if an item is for sale
bool purchased; //true if an item has been purchased
```

### String literals
```sol
String shipped = "shipped"; // shipped
String delivered = 'delivered'; // delivered
String newItem = "newItem"; // newItem
```
\<newline> escapes a new line
\n new line
\r carriage return
\t tab

### Address
* An address is a type with a 20-byte value that represents an Ethereum user account. This type can either be a regular address or an address payable.
* you can only send to address payable
```sol
address payable public seller; // account for the seller
address payable public buyer; // account for the user

function transfer(address buyer, uint price) {
    buyer.transfer(price); // the transfer member transfers the price of the item
}
```
### Enums
```sol
enum Status { 
    Pending,
    Shipped,
    Delivered 
}

Status public status;

constructor() public {
    status = Status.Pending;
}
```

## Explore reference types
* reference types provide a data location for the value.

### Data location

memory:
* The location where function arguments are stored
* Has a lifetime limited to the lifetime of an external function call
storage:
* The location where state variables are stored
* Has a lifetime limited to the contract lifetime
calldata:
* The location where function arguments are stored
* This location is required for parameters of external functions, but can also be used for other variables
* Has a lifetime limited to the lifetime of an external function call
```sol
contract C {

  uint[] x;
  
  // the data location of values is memory
  function buy(uint[] memory values) public {
      x = values; // copies array to storage
      uint[] storage y = x; //data location of y is storage
      g(x); // calls g, handing over reference to x
      h(x); // calls h, and creates a temporary copy in memory
  }

  function g(uint[] storage) internal pure {}
  function h(uint[] memory) public pure {}
}
```

### Arrays
```sol
uint[] itemIds; // Declare a dynamically sized array called itemIds
uint[3] prices = [1, 2, 3]; // initialize a fixed size array called prices, with prices 1, 2, and 3
uint[] prices = [1, 2, 3]; // same as above
```

length: Get the length of an array.
push(): Append an element at the end of an array.
pop: Remove an element from the end of an array.

### Structs
```sol
struct Items_Schema {
    uint256 _id;
    uint256 _price;
    string _name;
    string _description;
}
```

### Mapping types
* however it seems you can update the keys one at a time
```sol
contract Items {
    uint256 item_id = 0;

    mapping(uint256 => Items_Schema) public items;

    struct Items_Schema {
      uint256 _id:
      uint256 _price:
      string _name;
    }

    function listItem(uint 256 memory _price, string memory _name) public {
      item_id += 1;
      items[vehicle_id] = Items_Schema(item_id, _price, _name);
    }
}
```

### Challenge
* Create a contract where the balances for the buyer gets initaited so if you provide an address a buyer can start buying things

* in remix ide what you need to do head to file explorer make a new file
* look for the solidilty compiler icon and click it compile w/o warnings
* click on the deploy contract and get that to run
```sol
pragma solidity >0.8.0;

contract Marketplace {
    address public seller;
    address  public buyer;
    mapping (address => uint) public balances;

    event ListItem(address seller, uint price);
    event PurchasedItem(address seller, address buyer, uint price);

    enum StateType {
          ItemAvailable,
          ItemPurchased
    }

    StateType public State;

    constructor() public {
        seller = msg.sender;
        State = StateType.ItemAvailable;
    }

    function buy(address seller, address buyer, uint price) public payable {
        require(price <= balances[buyer], "Insufficient balance");
        State = StateType.ItemPurchased;
        balances[buyer] -= price;
        balances[seller] += price;

        emit PurchasedItem(seller, buyer, msg.value);
    }

    function inititateBalanaceForBuyer  (uint fundAmount,address buyer) public{
        balances[buyer] = fundAmount;
    }
}
```

#  Write Ethereum smart contracts by using Solidity 
* theres tools out there

## What is a smart contract?
__Transparency__: Blockchain users can read smart contracts and can access them by using APIs.
__Immutability__: Smart contract execution creates logs that can't be changed.
__Distribution__: The output of the contract is validated and verified by nodes on the network. Contract states can be publicly visible. In some cases, even "private" variables are visible.

## Frameworks
* Open Zeppelin, Truffle Suite, Truffle for vscode extension


# Questions
* does the transaction trigger the smart contract to make updates