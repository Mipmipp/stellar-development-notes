# Stellar doc

# **Create Account**

1. **Keypair Initialization:**
    - `questKeypair` holds the secret key for your existing Stellar Quest account.
    - `newKeypair` generates a new random keypair for the account you're creating.
2. **Server and Network:**
    - `server` connects to the Stellar testnet using `<https://horizon-testnet.stellar.org>`.
    - `questAccount` retrieves details of your existing account using the quest keypair's public key.
    - The network passphrase is set to `Networks.TESTNET`.
3. **Transaction Building:**
    - `TransactionBuilder` is used to construct the transaction with:
        - Source account: `questAccount` (implicitly set as source with no `source` field)
        - Fee: `BASE_FEE` (minimum required fee)
        - Network passphrase: `Networks.TESTNET`
4. **Create Account Operation:**
    - This operation uses the `addOperation` method on the `transaction` object.
    - It specifies:
        - `destination`: The public key of the new account (`newKeypair.publicKey()`)
        - `startingBalance`: The initial XLM amount sent to the new account ("1000" lumens in this case)
5. **Transaction Finalization and Submission:**
    - `transaction.setTimeout(30)` sets a 30-second timeout for transaction validity.
    - `transaction.sign(questKeypair)` signs the transaction with the quest keypair (authorization).
    - `server.submitTransaction(transaction)` sends the signed transaction to the testnet for processing.
    

```tsx
const {
  Keypair,
  Server,
  TransactionBuilder,
  Networks,
  Operation,
  BASE_FEE
} = require('stellar-sdk')

const questKeypair = Keypair.fromSecret('SECRET_KEY_HERE')
const newKeypair = Keypair.random()

// You would need to remove the '-testnet' here, if you were using the Stellar Public network.
const server = new Server('https://horizon-testnet.stellar.org')
const questAccount = await server.loadAccount(questKeypair.publicKey())

let transaction = new TransactionBuilder(
  questAccount, {
    fee: BASE_FEE,
    networkPassphrase: Networks.TESTNET
  })
  .addOperation(Operation.createAccount({
    destination: newKeypair.publicKey(),
    startingBalance: "1000" // You can make this any amount you want (as long as you have the funds!)
  })
  .setTimeout(30)
  .build()
transaction.sign(questKeypair)

await server.submitTransaction(transaction)
```

# **Payment**

1. **Payment Operation:**
- `destination`: Recipient of the payment
- `asset`: Type of asset being sent (XLM for this quest)
- `amount`: Quantity of the asset to send
- `source`: Account providing the funds (optional, defaults to transaction source)

```tsx
const {
  Keypair,
  Server,
  TransactionBuilder,
  Networks,
  Operation,
  Asset,
  BASE_FEE
} = require('stellar-sdk')

const { friendbot } = require('@runkit/elliotfriend/sq-learn-utils/1.0.6')

const questKeypair = Keypair.fromSecret('SECRET_KEY_HERE')
const destinationKeypair = Keypair.random()

await friendbot([questKeypair.publicKey(), destinationKeypair.publicKey()])

const server = new Server('https://horizon-testnet.stellar.org')
const questAccount = await server.loadAccount(questKeypair.publicKey())

const transaction = new TransactionBuilder(
  questAccount, {
    fee: BASE_FEE,
    networkPassphrase: Networks.TESTNET
  })
  .addOperation(Operation.payment({
    destination: destinationKeypair.publicKey(),
    asset: Asset.native(),
    amount: '100'
  }))
  .setTimeout(30)
  .build()

transaction.sign(questKeypair)

await server.submitTransaction(transaction)
```

# Trustline and New Asset

**1. Key Roles:**

- This script establishes a trustline for a custom asset called "POULZ" on the Stellar testnet.

**2. Key Roles:**

- **Quest Account:** The account requesting the trustline (represented by `questKeypair`).
- **Issuer Keypair:** The account issuing the "POULZ" asset (generated randomly in this example).

**3. Trustline Establishment:**

- The `changeTrust` operation is used to create the trustline for the "POULZ" asset.
- The `asset` field defines the custom asset by specifying its code ("POULZ") and issuer (`issuerKeypair.publicKey()`).
- The `limit` field sets the maximum amount of "POULZ" the Quest Account can receive (limited to "100" in this case).
- The `source` field identifies the Quest Account (`questKeypair.publicKey()`) as the entity requesting the trustline.

**4. Asset Information:**

- The `poulzAsset` variable stores details about the custom asset including its code and issuer.
- This demonstrates creating an asset object that can be used in transactions.

**5. Stellar Network Interaction:**

- The script connects to the Stellar testnet server (`https://horizon-testnet.stellar.org`).
- The `questAccount` object retrieves account information for the Quest Account using its public key.
- The transaction is built, signed, and submitted to the network for processing.

**6. Additional Notes:**

- The maximum asset limit can be omitted for unlimited trust, or set to 0 to remove an existing trustline.
- Successful execution grants the Quest Account the ability to receive and hold "POULZ" tokens.

```tsx
const {
  Keypair,
  Server,
  TransactionBuilder,
  Networks,
  Operation,
  Asset,
  BASE_FEE
} = require('stellar-sdk')
const { friendbot } = require('@runkit/elliotfriend/sq-learn-utils/1.0.6')

const questKeypair = Keypair.fromSecret('SECRET_KEY_HERE')
const issuerKeypair = Keypair.random()

await friendbot([questKeypair.publicKey(), issuerKeypair.publicKey()])

const server = new Server('https://horizon-testnet.stellar.org')
const questAccount = await server.loadAccount(questKeypair.publicKey())

const poulzAsset = new Asset(
  code = 'POULZ',
  issuer = issuerKeypair.publicKey()
)

const transaction = new TransactionBuilder(
  questAccount, {
    fee: BASE_FEE,
    networkPassphrase: Networks.TESTNET
  })
  .addOperation(Operation.changeTrust({
    asset: poulzAsset,
    limit: “100”,
    source: questKeypair.publicKey()
  }))
  .setTimeout(30)
  .build()

transaction.sign(questKeypair)

await server.submitTransaction(transaction)
```
