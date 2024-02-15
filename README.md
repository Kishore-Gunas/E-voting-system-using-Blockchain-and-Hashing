# E-voting-system-using-Blockchain-and-Hashing

#### Introduction
Electronic voting systems must prioritize security to ensure accessibility for voters while safeguarding against external interference that could alter votes or compromise the integrity of a voter’s ballot. Although many systems use techniques to conceal voter identities, these methods do not guarantee complete anonymity or protection, as intelligence agencies worldwide have the capability to monitor and potentially intercept votes due to their control over various parts of the Internet.

#### Blockchain
Blockchain was first introduced by Satoshi Nakamoto (a pseudonym), who proposed a peer-to-peer payment system that allows cash transactions through the Internet without relying on trust or the need for a financial institution.

Blockchain is an ordered data structure that contains blocks of transactions. Each block in the chain is linked to the previous block in the chain. The first block in the chain is referred to as the foundation of the stack. Each new block created gets layered on top of the previous block to form a stack called a Blockchain. 

All of the magic lies in the way this data is stored and added to the blockchain. A blockchain is essentially a linked-list containing ordered-data, with some constraints like below:

* Blocks can't be modified once added; in other words, it is "append-only."
* There are specific rules for appending data to it.
* It's distributed in architecture.
* Enforcing these constraints yields some highly desirable characteristics:

* Immutability and durability of data
* No single point of control or failure
* A verifiable audit trail of the order in which data was added

Each block in the stack is identified by a hash placed on the header. This hash is generated using the Secure Hash Algorithm (SHA-256) to generate an almost idiosyncratic fixed-size 256-bit hash.

Each header contains information that links a block to its previous block in the chain, which creates a chain linked to the very first block ever created, which is referred to as the foundation. The primary identifier of each block is the encrypted hash in its header. A digital fingerprint that was made combining two types of information: the information concerning the new block created, as well as the previous block in the chainAs soon as a block is created, it is sent over to the Blockchain. The system will keep an eye on incoming blocks and continuously update the chain when new blocks arrive.

#### Proposed System


1. **Authentication**:
   - The e-Voting system should allow only **pre-registered voters** to cast their votes. Since our system won't support a registration process, we need to verify voters' identities against a previously verified database.
   - This ensures that only authorized individuals participate in the election, and each voter can cast their vote **only once**.

2. **Anonymity**:
   - Anonymity is crucial. The e-Voting system must ensure that there are **no links between voters' identities and their ballots**.
   - Voters should remain anonymous both during and after the election process.

3. **Accuracy**:
   - Ensuring accurate votes is paramount. Every vote should be counted correctly, and no changes, duplications, or removals should occur.

4. **Verifiability**:
   - The system should be **verifiable** to ensure that all votes are counted correctly.
   - Blockchain technology, with its decentralized and tamper-resistant nature, provides a reliable platform for secure voting.

#### CONCLUSION

This proposal outlines an electronic voting system leveraging **Blockchain technology**. The Blockchain will be designed to be **publicly verifiable** and distributed in a manner that prevents corruption or tampering by any single entity.


<hr style="border:2px solid blue"> </hr>

# Implementation of Blockchain in E-voting system

## About Application
Our goal is to create an application that enables voters to cast their votes for their preferred political party using their unique Voter ID. Here are the key features we’ll focus on:

Voter Authentication and Voting:
Only registered voters with valid Voter IDs will be allowed to participate.
Each voter can cast their vote only once using their unique Voter ID.
Blockchain-Based Voting:
We’ll leverage blockchain technology to store voting information securely.
Votes will be recorded on the blockchain, ensuring immutability and permanence.
User Interface:
Users will interact with the application through a simple web interface.
The interface will facilitate voting and display relevant information.
Now, let’s delve into the details of how we’ll structure the data within the blockchain to achieve these goals.

* voter_id (Voter with their voter ID)
* party (Political parties, Leader)
* timestamp

We'll be storing data in our blockchain in a format that's widely used: JSON. Here's what a post stored in blockchain will look like:

```
"transactions": [
        {
          "voter_id": "VOTER1",
          "party": "Democratic Party",
          "timestamp": 14864.56
        }
      ],
```

In a blockchain, transactions are grouped into blocks. Each block can contain one or multiple transactions. These blocks are generated frequently and added to the blockchain. Each block has a unique identifier, ensuring that data remains secure, tamper-resistant, and consistent. It’s a reliable way to store information, whether for voting or other applications.
```
class Block:
    def __init__(self, index, transactions, timestamp, previous_hash, nonce=0):
        self.index = index
        self.transactions = transactions
        self.timestamp = timestamp
        self.previous_hash = previous_hash
        self.nonce = nonce
```

### Structure of Blockchain

### 1. Digital fingerprints to the blocks
In our project, we want to ensure that the data stored inside each block remains tamper-proof. To achieve this, we use cryptographic hash functions.

Here’s how it works:

A hash function takes data of any size and produces a fixed-size output (called a hash).
This hash acts like a digital fingerprint or signature for the input data.
In our case, we use the SHA-256 hash function.
We store this hash value inside our Block object to verify the integrity of the data within the block.
By doing this, we can detect any unauthorized changes to the data stored in the blockchain. 🛡️🔒
```
def compute_hash(self):
        """
        A function that return the hash of the block contents.
        """
        block_string = json.dumps(self.__dict__, sort_keys=True)
        return sha256(block_string.encode()).hexdigest()
```
### 2. Chain the blocks

In a blockchain, ensuring the integrity of the entire chain is crucial. Here's how it works:

1. **Chaining Blocks**:
   - Each block in the blockchain is linked to the previous block using a field called `previous_hash`.
   - This chaining creates a dependency among consecutive blocks.
   - If any content in a previous block changes:
     - The hash of that block would change.
     - This mismatch would affect the `previous_hash` field in the next block.
     - Since the hash computation includes the `previous_hash`, the next block's hash would also change.
   - Ultimately, any altered block invalidates the entire chain following it.

2. **Genesis Block**:
   - The very first block in the blockchain is called the **genesis block**.
   - It can be generated manually or through unique logic.
   - The `previous_hash` for the genesis block is typically set to a special value (e.g., all zeros).

By maintaining this chain of dependencies, the blockchain ensures data consistency and security. 🛡️🔗

### 3. Blockchain Proof of work

1. **Proof of Work (PoW)**:
   - PoW is a method used in blockchain networks (like Bitcoin) to confirm transactions and create new blocks.
   - Miners compete to complete transactions (called mining).
   - When a miner successfully creates a valid block, they receive a reward.

2. **Ensuring Blockchain Security**:
   - If we change a previous block, we can easily recompute the hashes of subsequent blocks and create a different valid blockchain.
   - To prevent this, we add a constraint to the hash: it must start with a certain number of leading zeroes.
   - We introduce a new field called **nonce** in each block.
   - Miners keep changing the nonce until they find a hash that meets the constraint.
   - The more leading zeroes required, the harder it is to find the right nonce.

In summary, PoW ensures security by making hash calculations difficult and random, and the nonce serves as proof that miners performed the necessary computations. 🛡️⛏️

### 4. Add blocks to the chain
Certainly! Let's simplify it:

When adding a block to the chain, we need to verify two things:

1. **Data Integrity**:
   - We ensure that the data is untampered. This involves checking if the provided Proof of Work (PoW) is correct.

2. **Transaction Order**:
   - We preserve the order of transactions. The `previous_hash` field of the block being added should point to the hash of the latest block in our chain.

By verifying these aspects, we maintain the integrity and consistency of the blockchain. 🛡️🔗

### 5. Mining

**Mining** in blockchain technology involves adding transactions to the distributed public ledger (the blockchain). Here's how it works:

1. **Unconfirmed Transactions**:
   - Initially, transactions are stored in a pool of unconfirmed transactions.
   - These transactions need to be organized into blocks.

2. **Mining Process**:
   - Miners perform the process of putting unconfirmed transactions into a block.
   - They also compute the **Proof of Work** (PoW) for the block.
   - Finding a nonce that satisfies the constraints serves as proof that the block has been mined.

3. **Adding to the Chain**:
   - Once a valid nonce is found, the block can be added to the blockchain.
   - This ensures the integrity and security of the entire chain.

In summary, mining ensures that transactions are securely added to the blockchain. ⛏️🔗

## Instructions to run the application

Clone the project,

```sh
$ git clone 
```

# Windows users can follow this: 

Open Command Prompt and run the following commands

git clone 

Install the dependencies,

```sh
$ cd E-voting-system-using-blockchain-and-hashing
$ pip install -r requirements.txt
```

# Windows users can follow this: 

Open Command Prompt and run the following commands

Go to the project folder using cd command, then run this

pip install -r requirements.txt

Start a blockchain node server,

```sh

$ flask run --port 8000
```
# Windows users can follow this: 

Open Command Prompt and run the following commands

set FLASK_APP=service.py

flask run --port 8000

You can always change the port as you wish, but change it in all respective places such as app.py and service.py files

One instance of our blockchain node is now up and running at port 8000.


Run the application on a different terminal session,

```sh
$ python app.py
```

# Windows users can follow this: 

Open Command Prompt and run the following commands

python app.py

The application should be up and running at http://localhost:8000.

