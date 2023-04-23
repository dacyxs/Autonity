# Onboard validator<br>
# Register as a Validator<br>
## Prerequisites <br>

To register a validator you need:<br>

1)A running instance of the Autonity Go Client running on your host machine. This will be the nod to be registered as a validator.<br>
2)A configured instance of aut .<br>
3)An account that has been funded with auton (to pay for transaction gas costs). Note that this account will become the validator’s treasury account - the account used to manage the validator, that that will also receive the validator’s share of staking rewards.<br>
4)This guide also assumes that the command-line JSON processor jq is available<br>

## Scoring Rule
Every registered participant in this task gets an easy 50 points for managing to get their validator node registered on-chain. <br>
For the rest of the task, you will get points proportional to your node’s uptime in the tendermint consensus algorithm. <br>
Your node gets an uptime measure for each epoch, and for each epoch where this measurement is ≥90%, you get 1 point. <br>
Epochs are ≈30min each, so if Round #2 lasts 3-weeks, a node that performs perfectly could earn ≈1008 points. <br>

Points will be allocated daily, after 00:00:00 UTC. Per-task and overall scoreboards are available https://validators.game.autonity.org/dashboards/ <br>

Note that if a validator is onboarded before the Round start time the 50 points earned for registration will show on the scoreboard. Points for the rest of the task will only be scored between the Round’s start and end date time. <br>


# Step 1. Generate a cryptographic proof of node ownership 
This must be performed on the host machine running the Autonity Go Client, using the autonity genEnodeProof command:<br>
<NODE_KEY_PATH> should be changed with the node key path that created in previous steps. <br>
<TREASURY_ACCOUNT_ADDRESS> should be changed with your account address <br>
If you already followed my previous documentation you can skip to next code.

```
autonity genEnodeProof --nodekey <NODE_KEY_PATH> <TREASURY_ACCOUNT_ADDRESS>
```

This code will be enough to get signature text.
<TREASURY_ACCOUNT_ADDRESS> should be changed with your account address <br>

```
docker run -t -i --volume $(pwd)/autonity-chaindata:/autonity-chaindata --name autonity-proof --rm ghcr.io/autonity/autonity:latest genEnodeProof --nodekey ./autonity-chaindata/autonity/nodekey <TREASURY_ACCOUNT_ADDRESS>
```

You should see something like this(We will use signature text later. Be sure to save it):

![image](https://user-images.githubusercontent.com/106930902/233868401-7b939b16-1a79-4382-9140-78cbc54483ba.png)

# Step 2. Determine the validator enode and address 
```
aut node info
```
The url is returned in the admin_enode field.
```
aut validator compute-address <admin_enode>
```
Make a note of this identifier that return to you.
![image](https://user-images.githubusercontent.com/106930902/233868590-7a9c2c15-a421-4837-993c-7d87bde03b2e.png)

# Step 3. Submit the registration transaction. 

<ENODE_URL>: the enode url returned in Step 2.<br>
<PROOF>: the proof of enode ownership generated in Step 1.<br>

```
aut validator register <ENODE_URL> <PROOF> | aut tx sign - | aut tx send -
```

Once the transaction is finalized (use aut tx wait <txid> to wait for it to be included in a block and return the status), the node is registered as a validator in the active state. It will become eligible for selection to the consensus committee once stake has been bonded to it.

# Step 4. Confirm registration
```  
aut validator list
```
  
Confirm the validator details using:
  
  aut validator info --validator 0x49454f01a8F1Fbab21785a57114Ed955212006be

  
  Validator registration form
  https://game.autonity.org/incentive-game-forms-frontend/validator.html
  
  
  
  
