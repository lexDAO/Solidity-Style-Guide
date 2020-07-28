# Solidity Style Guild
solidity style tips for legal engineering drafting

## Events

Events should be ordered in logical flow or chronology, as they often can paint a picture of how deals progress:

    event RegisterLocker(address indexed client, address[] indexed provider, address indexed resolver, address token, uint256[] amount, uint256 cap, uint256 index, uint256 termination, bytes32 details);	
    event DepositLocker(uint256 indexed index, uint256 indexed cap);  
    event Release(uint256 indexed index, uint256[] indexed milestone); 
    event Withdraw(uint256 indexed index, uint256 indexed remainder);
    event Lock(address indexed sender, uint256 indexed index, bytes32 indexed details);
    event Resolve(address indexed resolver, uint256 indexed clientAward, uint256 indexed providerAward, uint256 index, uint256 resolutionFee, bytes32 details);

## Function Variables

Function variables should be stacked together per type, to prep gas-saving habit of "[variable packing](https://mudit.blog/solidity-gas-optimization-tips/)." Logical, and if possible, alphabetic order is preferred for consistency. Header comment to explain purpose and expected outcome of function is also recommended:

        function registerLocker( // register locker for token deposit and client deal confirmation
            address client,
            address[] calldata provider,
            address resolver,
            address token,
            uint256[] calldata amount, 
            uint256 cap,
            uint256 milestones,
            uint256 termination,
            bytes32 details)
