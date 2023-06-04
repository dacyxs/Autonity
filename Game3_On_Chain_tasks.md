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

# In order to continue your current block have to be equal to highestBlock

![image](https://user-images.githubusercontent.com/106930902/235779105-e9e30aa4-e855-44e7-8c79-aecc4c72b775.png)

# Important Note: We will start with doing same as do in chain tasks in order to set variables again. Also, step 2 is important to get your validator address. 

## Step 1. Generate a cryptographic proof of node ownership 
This must be performed on the host machine running the Autonity Go Client, using the autonity genEnodeProof command:<br>
<TREASURY_ACCOUNT_ADDRESS> should be changed with your account address <br>
Get your signature hex with below command

```
SIGN_ADDR=$(autonity genEnodeProof --nodekey autonity-chaindata/autonity/nodekey <TREASURY_ACCOUNT_ADDRESS> | awk '{print $3}')
```
```
echo "Signature Address: " $SIGN_ADDR
```

## Step 2. Determine the validator enode and address 
```
ENODEURL=$(aut node info -r http://127.0.0.1:8545 | grep enode | awk '{print $2}' | tr -d ,'"')
```
```
echo $ENODEURL
```
The url is returned in the admin_enode field with echo line.
```
VALIDATOR=$(aut validator compute-address $ENODEURL)
```

Make a note of this identifier that return to you. This is the unique code for your validator. 
![image](https://user-images.githubusercontent.com/106930902/233868590-7a9c2c15-a421-4837-993c-7d87bde03b2e.png)

## Step 3. Submit the registration transaction. 

<ENODE_URL>: the enode url returned in Step 2.<br>
<PROOF>: the proof of enode ownership generated in Step 1.<br>

```
REGS_VALI=$(aut validator register $ENODEURL $SIGN_ADDR | aut tx sign - | aut tx send -)
```
```
echo "Validator Registration TX : " $REGS_VALI
```
  
Once the transaction is finalized (use aut tx wait <txid> to wait for it to be included in a block and return the status), the node is registered as a validator in the active state. It will become eligible for selection to the consensus committee once stake has been bonded to it.

## Step 4. Confirm registration
```  
aut validator list
```
  
Confirm the validator details using:

```
aut validator info --validator $VALIDATOR
```

## Step 5. Create new keyfile from nodekey to sign new message validator onboarded. Second step change newkeyfilename with the filename created by first step below. Use created signature in the form.
```
aut account import-private-key autonity-chaindata/autonity/nodekey
```
  ```
  aut account sign-message "validator onboarded" --keyfile .autonity/keystore/<newkeyfilename>
  ```
  
Validator registration form(if you already participated to game2 you can skip this form.)
https://game.autonity.org/incentive-game-forms-frontend/validator.html
  
# Follow me @
https://twitter.com/Dacxys

  
  

