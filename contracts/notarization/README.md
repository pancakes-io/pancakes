## Notarization (& Proof of Existence)

```solidity
pragma solidity ^0.4.4;


contract Notary {

  struct Record {
      uint mineTime;
      uint blockNumber;
  }

  mapping (bytes32 => Record) private docHashes;

  function Notary() public {
    // constructor
  }

  function addDocHash (bytes32 hash) public {
    Record memory newRecord = Record(now, block.number);
    docHashes[hash] = newRecord;

  }

  function findDocHash (bytes32 hash) public constant returns(uint, uint) {
    return (docHashes[hash].mineTime, docHashes[hash].blockNumber);
  }

}
```

[Notary.sol](https://github.com/stbeyer/docCertTutorial/blob/master/contracts/Notary.sol "Notary.sol") (license: MIT) by [Stefan Beyer](https://github.com/stbeyer "Stefan Beyer")

```solidity
pragma solidity ^0.5.0;
contract ProofOfExistence {
    
    event ProofCreated(
        uint256 indexed id,
        bytes32 documentHash
    );
    address public owner;
  
    mapping (uint256 => bytes32) hashesById;
    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner is allowed to access this function.");
        _;
    }
    modifier noHashExistsYet(uint256 id) {
        require(hashesById[id] == "", "No hash exists for this id.");
        _;
    }
    constructor() public {
        owner = msg.sender;
    }
    function notarizeHash(uint256 id, bytes32 documentHash) public onlyOwner noHashExistsYet(id) {
        hashesById[id] = documentHash;
        emit ProofCreated(id, documentHash);
    }
    function doesProofExist(uint256 id, bytes32 documentHash) public view returns (bool) {
        return hashesById[id] == documentHash;
    }
}
```

[ProofOfExistence.sol](https://github.com/ramyhardan/proof-of-existence/blob/master/contracts/ProofOfExistence.sol "ProofOfExistence.sol") (license: MIT) by [Ramy Hardan](https://github.com/ramyhardan "Ramy Hardan")