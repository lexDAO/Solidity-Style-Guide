# Solidity Style Guide
solidity style tips for legal engineering drafting

## Events

Events should be ordered in logical flow or chronology, as they often can paint a picture of how deals progress:

    event RegisterLocker(address indexed client, address[] indexed provider, address indexed resolver, address token, uint256[] amount, uint256 cap, uint256 index, uint256 termination, bytes32 details);	
    event DepositLocker(uint256 indexed index, uint256 indexed sum);  
    event Release(uint256 indexed index, uint256[] indexed milestone); 
    event Withdraw(uint256 indexed index, uint256 indexed remainder);
    event Lock(address indexed sender, uint256 indexed index, bytes32 indexed details);
    event Resolve(address indexed resolver, uint256 indexed clientAward, uint256 indexed providerAward, uint256 index, uint256 resolutionFee, bytes32 details);

Grouping the objects within an event in a logical order also makes a big difference and can reduce gas costs. Deciding what objects get emitted as part of an event is also an important consideration for providing the legal engineer with the right information and also constructing a full-stack deal application where the front-end can easily grab event objects either through listeners or a subgraph. 

When emiting an event from a function, it's preferable if the event goes at the end of the function, or right before the return, if applicable. Sometimes the event will need to go higher up in the function to avoid stack to deep issues. 

    function submitWhitelistProposal(address tokenToWhitelist, bytes32 details) external returns (uint256 proposalId) {
        require(tokenToWhitelist != address(0), "need token");
        require(!tokenWhitelist[tokenToWhitelist], "already whitelisted");
        require(approvedTokens.length < MAX_TOKEN_WHITELIST_COUNT, "whitelist maxed");

        uint8[7] memory flags; // [sponsored, processed, didPass, cancelled, whitelist, guildkick, action]
        flags[4] = 1; // whitelist

        _submitProposal(address(0), 0, 0, 0, tokenToWhitelist, 0, address(0), details, flags);
        
        return proposalCount - 1;
    }
    
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
## Structs

The objects included in structs should also be grouped logically and by type in order to save on gas costs. We recommend starting with addresses, then uints, then bytes and bools. 

    struct Action {
        address proposer;
        address to;
        uint256 value;
        bytes data;
    }
