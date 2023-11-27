# Evoting

Project made by: 
- [Safa Laabidi](https://github.com/safa-abidi) 
- [Ines Achour](https://github.com/inesachour)
- [Amal Sammari](https://github.com/Amal1999)
<br/>
<br/>

The project contains these sections:<br/>
[I- Introduction](https://github.com/safa-abidi/Lab2-Eth-Evoting#i-introduction)<br/>
[II- Implementation](https://github.com/safa-abidi/Lab2-Eth-Evoting#ii--implementation)<br/>
&ensp;[1- Set Up](https://github.com/safa-abidi/Lab2-Eth-Evoting#1--set-up)<br/>
&ensp;[2- Project Implementation](https://github.com/safa-abidi/Lab2-Eth-Evoting#2--project-implementation)<br/>
&ensp;&ensp;[A- Smart Contract](https://github.com/safa-abidi/Lab2-Eth-Evoting#a--smart-contract)<br/>
&ensp;&ensp;[B- Migration](https://github.com/safa-abidi/Lab2-Eth-Evoting#b--migration)<br/>
&ensp;&ensp;[C- Testing](https://github.com/safa-abidi/Lab2-Eth-Evoting#c--testing)<br/>
&ensp;&ensp;[D- User Interface](https://github.com/safa-abidi/Lab2-Eth-Evoting#d--user-interface)<br/>
&ensp;&ensp;[E- MetaMask](https://github.com/safa-abidi/Lab2-Eth-Evoting#e--metamask)<br/>
&ensp;&ensp;[F- Testing the Evoting Application](https://github.com/safa-abidi/Lab2-Eth-Evoting#f--testing-the-evoting-application)<br/>
&ensp;&ensp;[G- More Candidates (4)](https://github.com/safa-abidi/Lab2-Eth-Evoting#g--adding-candidates-4)<br/>

<br/>

## I-Introduction
In this project we will be building a voting DApp.
We are going to use Truffle Framework, Ganache (a local blockchain simulator) and MetaMask (a wallet).
We will also need NPM which come with Node.js. </br>
![Node Version](/screenshots/node_version.png "Node Version")

## II- Implementation
### 1- Set Up
we execute this command to get a basic project structure for our Evoting app :
```
 truffle unbox pet-shop 
```

### 2- Project Implementation

#### A- Smart Contract

We create a file named [Election.sol](https://github.com/safa-abidi/Lab2-Eth-Evoting/blob/main/contracts/Election.sol) in contracts directory

Let's break the code into pieces. <br/>
Starting by the first line inside the contract 

```
struct Candidate {
    uint id;
    string name;
    uint voteCount;
}
``` 
This structure represent and contains the data of our candidates.

```
mapping(address => bool) public voters;
mapping(uint = > Candidate) public candidates;
uint public candidatesCount;
```
In the previous code we created a mapping to store the voters and another for the candidates and we added a counter to keep track of the number of our candidates.

Now we add the event that will be triggered whenever there's a new vote and then we implement a function that will trigger this event :
```
event votedEvent (
    uint indexed _candidateId
);
function vote (uint _candidateId) public {
    require(!voters[msg.sender]);
    require(_candidateId > 0 && _candidateId <= candidatesCount);
    voters[msg.sender] = true;
    candidates[_candidateId].voteCount ++;
    emit votedEvent(_candidateId);
}
```

The function 'addCandidate' which is obviously used to add candidates that we will vote on later :
```
function addCandidate (string _name) private {
    candidatesCount ++;
    candidates[candidatesCount] = Candidate(candidatesCount, _name, 0);
}
```

Finally we call our 'addCandidate' function in the constructor in order to add automatically 2 candidates whenever we deploy the smart contract: 
```
constructor () public {
    addCandidate ("Candidate 1") ;
    addCandidate ("Candidate 2") ;
}
```

#### B- Migration
So we create a file named [2_deploy_contracts.js](https://github.com/safa-abidi/Lab2-Eth-Evoting/blob/main/migrations/2%20deploy%20contracts.js) in migrations directory <br/>

Now we need to launch Ganache and then run the migrations :
```
truffle migrate
```
![Migration](/screenshots/migration1.png "Migration")
![Migration](/screenshots/migration2.png "Migration")

After the migration is done we can see the created blocks in ganache :
![Migration](/screenshots/ganache_after_migration1.png "Migration")
Those are the transactions :
![Migration](/screenshots/ganache_after_migration2.png "Migration")
And the contracts :
![Migration](/screenshots/ganache_after_migration3.png "Migration")

We can interact with our smart contract as following :
![Smart Contract](/screenshots/smart_contract_details.png "Smart Contract")

#### C- Testing
So we create a file named [election.js](https://github.com/safa-abidi/Lab2-Eth-Evoting/blob/main/test/election.js) in test folder <br/>

To run the test we use this truffle command :
```
truffle test
```
![Test](/screenshots/test2.png "Test")

#### D- User Interface
To create the UI of our application we need to change the code in [index.html](https://github.com/safa-abidi/Lab2-Eth-Evoting/blob/main/src/index.htmls) file in src/ directory

and then replace the code in [app.js](https://github.com/safa-abidi/Lab2-Eth-Evoting/blob/main/src/js/app.js) file located in the src/js directory

Finally we run this command to start the server :
```
npm run dev
```
![User Interface](/screenshots/user_interface_2c.png "User Interface")

We can see that the application is displaying 'Your Account: null' and that's because we need to connect MetaMask and Ganache.

#### E- MetaMask
The next step is connecting Ganache in MetaMask :
![MetaMask](/screenshots/metamsk_account.png "MetaMask")

#### F- Testing the Evoting Application
After the connection is made, we now able to choose a candidate to vote :
![Voting](/screenshots/vote_screen.png "Voting")
</br>
When clicking on 'vote' button, MetaMask shows this popup to confirm the transaction :
![Transaction](/screenshots/transaction.png "Transaction")
</br>
After confirming, we can see that the transaction is now pending :
</br>
![Pending Transaction](/screenshots/transaction2.png "Pending Transaction")
</br>
It might take some time before the transaction is made. 
Our vote is now casted :
![Vote](/screenshots/vote_succes_2c.png "Vote")
![Transaction](/screenshots/transaction_success_2c.png "Transaction")

#### G- More Candidates (4)
In order to add more candidates in our vote we just have to modify the constructor code in the 'Election.sol' Smart Contract to add as much candidates as we want :
```
constructor () public {
    addCandidate ("Candidate 1");
    addCandidate ("Candidate 2");
    addCandidate ("Candidate 3");
    addCandidate ("Candidate 4");
}
```

Now we use this command to launch the migration
```
truffle migrate --reset
```
After that, we can see that now we have 4 candidates :
![User Interface](/screenshots/user_interface_4c.png "User Interface")
</br>
Here we voted for the first candidate :
![Vote](/screenshots/vote_success_4c.png "Vote")

