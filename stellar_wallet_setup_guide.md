# Guide to Setting Up Stellar Wallets for Issuer and Distributor
This guide will help you set up Stellar wallets for both an issuer and a distributor. Follow the steps below to create accounts, establish a trustline, and make a payment.

## Step 1: Create Stellar Accounts
First, you'll need to create two Stellar accounts: one for the issuer and one for the distributor.

Navigate to the Stellar Laboratory Account Creator by [clicking here](https://laboratory.stellar.org/#account-creator?network=test).
Generate two keypairs, which will serve as the issuer and distributor accounts.
Use the provided options to fund both accounts.

## Step 2: Establish a Trustline
Next, set up a trustline between the distributor and an asset issued by the issuer.

Access the Transaction Builder by [clicking here](https://laboratory.stellar.org/#txbuilder?params=eyJhdHRyaWJ1dGVzIjp7ImZlZSI6IjEwMCIsImJhc2VGZWUiOiIxMDAiLCJtaW5GZWUiOiIxMDAifSwiZmVlQnVtcEF0dHJpYnV0ZXMiOnsibWF4RmVlIjoiMTAwIn19&network=test).
Use the distributor's public key as the source account.
Select "Change Trust" from the operations menu.
Specify the asset type as alphanumeric 4, use "PLZC" for the asset code, and enter the issuer's public key.

## Step 3: Make a Payment
Finally, you'll make a payment from the issuer to the distributor using the newly created asset.

Return to the Transaction Builder by [clicking here](https://laboratory.stellar.org/#txbuilder?params=eyJhdHRyaWJ1dGVzIjp7ImZlZSI6IjEwMCIsImJhc2VGZWUiOiIxMDAiLCJtaW5GZWUiOiIxMDAifSwiZmVlQnVtcEF0dHJpYnV0ZXMiOnsibWF4RmVlIjoiMTAwIn19&network=test).
Select "Payment" from the operations menu.
Enter the distributor's public key as the destination.
Specify the asset type as alphanumeric 4, use "PLZC" for the asset code, and provide the issuer's public key.
By following these steps, you'll have successfully created and configured Stellar wallets for an issuer and a distributor, and made an initial payment of the issued asset.
