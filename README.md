# Decentralized Autonomous Resolutions
Peer to peer dispute resolution.

## Description
A contract for dispute resolution.

#### Trust Issue
Disputes are resolved in an honest and fair way without 3rd party (corporate) interests influncing outcomes.

### How it Works
**Agreement is made - Offer accepted**

1. A `offeror` [opens](https://github.com/gumdropsteve/dispute-resolution/blob/main/contracts/Arbitrator.sol#L92-L110) an offer _Agreement_ (e.g. a rental contract stored on IPFS) to a `offeree`
2. The `offeree` accepts said _Offer_, and the _Agreement_ is set

**Something goes wrong - Dispute opened**

3. One of the parties (`offeror` or `offeree`) feels something went wrong, and would like to be compensated accordingly.
    - That party [opens](https://github.com/gumdropsteve/dispute-resolution/blob/main/contracts/Arbitrator.sol#L139-L176) a _Dispute_ and becomes the _Dispute_'s `plantiff`
    - The other party, now the _Dispute_'s `defendant`, is notified
4. The `defendant` has a few options: 1. Settle 2. Counter 3. Decline 
    - Settle: payment is transferred from the `defendant`'s deposit to the `plaintiff` and the _Dispute_ is over
    - Counter: a counter offer is submitted, and the `plaintiff` can either settle or decline
    - Decline: a voting period is opened so the _Dispute_ can be settled by vote
    
**They can't work it out - Peers vote**

5. Voters have staked `Token`, enabling them to vote on disputes, and are rewarded with `Token` for voting
    - `Token` is an [ERC-20](https://ethereum.org/en/developers/docs/standards/tokens/erc-20/) fungible token
    - All votes are FHE [encrypted](https://nodejs.org/api/crypto.html#crypto_crypto_hkdf_digest_key_salt_info_keylen_callback) through NuCypher
        - A private key (used for encrypting & decrypting the votes) is supplied by the `plaintiff` to the `defendant` as a dispute is opened
        - This private key is made public once the voting period has ended
    - Voters are rewarded for "good behavior" and disincentivized from "bad behavior"
        - Example: Alice rented Lee's apartment in San Clemente for a week and brought her dogs even though the agreement clearly stated "no pets allowed". Evidence includes videos from Ring of Alice walking in and out of the apartment with her 2 German Shepherds numerous times over the course of the week, including overnight stays.
        - Example "good behavior": voting Alice is in the wrong
        - Example "bad behavior": voting that Lee is in the wrong; manipulating the system to assist Alice or Lee
6. The voting period ends, the `plaintiff` and `defendant` decrypt and count the votes, and the private key is made public on chain
7. Once the votes have been tallied and verified by the `plaintiff` and `defendant`, payment (if applicable) is transferred from one party to the other
    - If the 2 parties disagree on the vote's results, that is something worth knowing

### Demo
#### Live Demo

#### Video

### Architecture diagram
![Architecture diagram](https://lh6.googleusercontent.com/cxdySU5sqAjYgaeh4c2xwz2E3LEObmPuNeC-SjIsV6nuXpMxHKBzyxJ9sT8qPvQwF0sraY_ocBwWs2LS7XhXnu-0BH6BisiKXjFuotg6psLBhzEBP1tUML69xLENj2B1lF-guVTq)

## Getting Started
### On SKALE
```
git clone https://github.com/gumdropsteve/dispute-resolution

cd dispute-resolution

npm install

truffle compile

truffle migrate --reset --network skale -f 2
```

#### Adding SKALE to Metamask
https://skale.network/docs/developers/wallets/getting-started#metamask
1. Add a Custom RPC Network
2. Network URL: https://eth-global-0.skalenodes.com:10456
3. Chain ID: 0xb9454a5c40f66

### Unit tests
To run the unit testing scripts
```
truffle test ./test/Arbitrator_test.js

truffle test ./test/Token_test.js
```

## Tech Used

<img align="right" width="150" height="150" src="https://lh5.googleusercontent.com/P2Fm4GklnCVSPuNnM2eLncdSahp0_pd7LzYJcK6tb-YIT0pdI6v9aElATV71ZGmdKhDsztXa0cW5pRCfmu0rempM5nXoBonM33DfCaUsJKCOQKBKKnwI2VJUqfT0IcJCobd_yreA">

#### IPFS
> A peer-to-peer hypermedia protocol designed to make the web faster, safer, and more open.
- [GitHub](https://github.com/ipfs/ipfs)
- [Website](https://ipfs.io/)
- We used IPFS to store _Agreement.documents_, _Dispute.plantiffEvidence_ and _Dispute.defendantEvidence_ files

<img align="right" width="150" height="150" src="https://lh6.googleusercontent.com/3WDXeY6cvDfW5-P6rmqtun9dRYYCtQa_c4MFqjNssE2CE4h2t8VfG5iHMADLNaX-Mq8kS7hQeEe99DV7lA-1tpCbtxirq6MFuMiJJQoSJU3vrCpNCuzLzbWWby2Ug7qAn9jfeVKt">

#### NuCypher
> Cryptographic Infrastructure for Privacy-Preserving Applications
> A decentralized threshold cryptography network offering interfaces and runtimes for secrets management and dynamic access control.
- [GitHub](https://github.com/nucypher/)
- [Website](https://www.nucypher.com/)
- We used NuCypher to help keep votes private while the voting period is live so that voters are not influenced by other voters, while allowing `plaintiff`s and `defendant`s to decrypt votes to see results

<img align="right" width="150" height="150" src="https://lh3.googleusercontent.com/YSzrZ4MAb3oDhGDo1d0yZ-ET8Bhb5b6RUbKJGXqKPMSFNEt8kKtqDQmyc7TZn6uQJllHQlU6VQxdt3uw2EW_RQEG6dU5py3d3VGcCtOY2U79rbHq5u4rpGFh8lBbnQQzDp7iLO34">

#### SKALE
Decentralized modular environment for Solidity dApps
- [GitHub](https://github.com/skalenetwork/)
- [Website](https://skale.network/)
- We used SKALE to eliminate gas fees and reduce costs


## Future Direction
This dispute resolution contract was origionally thought of as part of a decentralized real estate rental service, making AirBnb like disputes between guests and hosts more transparent and honest.
