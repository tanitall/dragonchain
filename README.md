# Dragonchain

The Dragonchain platform attempts to simplify integration of real business applications onto a blockchain. Providing features such as easy integration, protection of business data, fixed 5 second blocks, currency agnosticism, and interop features, Dragonchain shines a new and interesting light on blockchain technology.

#### No blockchain expertise required!

## Project Goals

1. Ease of integration of existing systems 
1. Ease of development for traditional engineers and coders unfamiliar with blockchain, 
distributed systems, and cryptography 
1. Client server style and simple RESTful integration points for business integration 
1. Simple architecture (flexible and usable for unforeseen applications) 
1. Provide protection of business data by default
1. Allow business focused control of processes
1. Fixed length period blocks 
1. Short/fast blocks 
1. Currency agnostic blockchain (multi­currency support) 
1. No base currency 
1. Interoperability with other blockchains public and private 
1. Adoption of standards as they become available (see ​W3C Blockchain Community 
Group blockchain standardization​) 


## Quick Links
* [Dragonchain Organization](https://dragonchain.github.io/)
* [Dragonchain Architecture Document](https://dragonchain.github.io/architecture) - [PDF](https://dragonchain.github.io/doc/DragonchainArchitecture.pdf)
* [Use Cases](https://dragonchain.github.io/blockchain-use-cases)
* [Disney Blockchain Standardization Notes](https://dragonchain.github.io/blockchain-standardization) - [W3C](https://github.com/w3c/blockchain/blob/master/standards.md)

## Support

* Slack Team: [Dragonchain Slack Team](https://dragonchain.slack.com/) sign up: [![Slack Status](https://dragonchain-slack.herokuapp.com/badge.svg)](https://dragonchain-slack.herokuapp.com)
* Slack Support Channel: [#support](https://dragonchain.slack.com/messages/support/)
* Email: support@dragonchain.org

## Maintainer
Joe Roets (j03)
joe@dragonchain.org

# Setup and Installation

### Running Stack via Docker
Requires Docker version 1.13.0 and docker-compose version 1.10.0 

Stack declaration is made via the docker-compose.yaml in the docker 
directory. Docker-compose.yml uses the dockerfile declarations that packages the project services into containers.

To bring up the stack:


    cd docker
    docker-compose up -d
Postgres is exposed at port 5432

Query service is exposed at port 80

Transaction service is exposed at port 81

Processor service is exposed at port 8080

### Database Setup

Requires Postgres 9.4+

    cd <Dragonchain Home>/sql
    createuser blocky
    createdb -O blocky blockchain
    psql -U blocky -d blockchain -a -f depl.sql

### Python Dependencies

Dragonchain utilized Python 2.7

To install Python library dependencies, run the following:

    pip install -r requirements.txt

### Code Generation

Message classes and RMI services are generated by thrift templates.
If you have Apache Thrift installed you can regenerate these classes by using the gen_thrift setup command:

    python setup.py gen_thrift

### Keys

    mkdir pki

* Signing Key Generation `openssl ecparam -name secp224r1 -genkey -out <Dragonchain Home>/pki/sk.pem`
* Verifying Key Generation `openssl ec -in pki/sk.pem -pubout -out <Dragonchain Home>/pki/pk.pem`
* **The signing key must be kept private.**  The pki directory is listed in .gitignore to prevent accidentally pushing keys to a public repository.

### Logs

Pre-create a directory for log files before execution.

    mkdir logs

# Execution

## Set the PYTHONPATH variable
    
    export PYTHONPATH=/path/to/dragonchain

## Transaction Service

    python <Dragonchain Home>/blockchain/transaction_svc.py --private-key <Dragonchain Home>/pki/sk.pem --public-key <Dragonchain Home>/pki/pk.pem
    
## Query Service

    python <Dragonchain Home>/blockchain/query_svc.py [-p port] //defaults to 8080

### Example Query

    localhost:8080/transaction/?create_ts=1475155787 //timestamp in Unix Epoch timecode
        - create_ts can be replaced with any of the header fields in a transaction
    
## Blockchain Processor

    python <Dragonchain Home>/blockchain/processing.py --private-key <Dragonchain Home>/pki/sk.pem --public-key <Dragonchain Home>/pki/pk.pem

    --private-key (required - signing key for transaction service and processing module)
    --public-key  (required)
    --host        (defaults to localhost)
    --port, -p    (defaults to 8080)
    --phase       (required)
    
    For phase 5 node only:
    --bitcoin-network (mainnet/testnet/regtest)
    
## Running a phase 5 node

The current implementation of the phase 5 node can connect to a local bitcoin node with wallet in order to run or
can remotely connect to another bitcoin wallet running Bitcoin-Qt. 
Make sure that your bitcoin.conf file is in the proper location:
    
##### Mac OSX

    $HOME/Library/Application Support/Bitcoin/
    
##### Linux

    $HOME/.bitcoin/
    
##### Windows
    
    %APPDATA%\Bitcoin\
    
#### bitcoin.conf file contents
    
    rpcuser="some username"
    rpcpassword="password for the node"
    
Optional for running on a remote node:
    
    rpcconnect="bitcoin_node_ip_address"
    rpcport="bitcoin_node_port" (defaults to 8333)
    

## Configuration

Configuration within yaml files within configs directory.

### Environment Variables

`BLOCKCHAIN_DB_HOSTNAME` = Hostname of the database instance (default localhost)

`BLOCKCHAIN_DB_PORT` = Port of the database server (default 5432)

`BLOCKCHAIN_DB_USERNAME` = Database username (default blocky)

`BLOCKCHAIN_DB_PASSWORD` = Database password (default None)

`BLOCKCHAIN_DB_NAME` = Blockchain database (default blockchain, also selects `.yaml` config and log file with same name)

# Contribution

Dragonchain uses a standard Feature Branch Workflow.

All feature development should take place in Git branch dedicated to that feature. A feature branch should be named starting with the ticket ID followed by a dash and a short description.

Issues are tracked within Github: [Dragonchain Issues](https://github.com/dragonchain/dragonchain/issues)

## Formatting

Code should follow the [PEP 8 Style Guide](https://www.python.org/dev/peps/pep-0008/) for Python code where possible. 

## Contributors

- [Joe Roets - Principal Architect / Vision](https://www.linkedin.com/in/j0j0r0)
- [Eileen Quenin - Product Manager / Evangelist](https://www.linkedin.com/in/eileenquenin)
- [Brandon Kite - Lead Developer](https://www.linkedin.com/in/bkite)
- [Dylan Yelton - Developer](https://www.linkedin.com/in/dylan-yelton-b11ba5aa)
- [Alex Benedetto - Developer](https://www.linkedin.com/in/alex-benedetto-6175048b)
- [Michael Bachtel - DevOps / Developer](https://www.linkedin.com/in/michael-bachtel-617b7b2)
- [Lucas Ontivero - Developer](https://ar.linkedin.com/in/lucasontivero)
- [Adam Bronfin - Developer / Reviewer](https://www.linkedin.com/in/adam-bronfin-694a7440)
- [Benjamin Israelson - Developer / Reviewer](https://www.linkedin.com/in/benjaminisraelson)
- [Forrest Fisher - Program Manager](https://www.linkedin.com/in/forrestfisher)
- [Robbin Schill - Program Manager](https://www.linkedin.com/in/robbin-schill-a798044)
- [Krassi Krustev - Developer](https://www.linkedin.com/in/krassimir-krustev-252483ab)
- [Rob Eickmann - iOS Developer](https://www.linkedin.com/in/roberte3)
- [Sean Ochoa - DevOps / Sysadmin](https://www.linkedin.com/in/seanochoa)
- [Paul Sonier - Developer / Reviewer](https://www.linkedin.com/in/paul-sonier-18135b2)
- [Kevin Schumacher - Artist / Web Design](https://www.linkedin.com/in/schubox)
- [Brian J Wilson - Architect](https://www.linkedin.com/in/brian-wilson-9325a776)
- [Mike De'Shazer - Developer / Reviewer](https://kr.linkedin.com/in/mikedeshazer)
- [Tai Kersten - Developer / Reviewer](https://kr.linkedin.com/in/tai-kersten-bb460412a/en)
- Steve Owens - Reviewer
- Mark LaPerriere - Reviewer
- Kevin Duane - Reviewer
- Chris Moore - Reviewer

# Disclaimer

The comments, views, and opinions expressed in this forum are those of the authors and do not necessarily reflect the official policy or position of the Walt Disney Company, Disney Connected and Advanced Technologies, or any affiliated companies.

All code contained herein is provided “AS IS” without warranties of any kind. Any implied warranties of non-infringement, merchantability, and fitness for a particular purpose are expressly disclaimed by the author(s).

# License

```
Copyright 2016 Disney Connected and Advanced Technologies

Licensed under the Apache License, Version 2.0 (the "Apache License")
with the following modification; you may not use this file except in
compliance with the Apache License and the following modification to it:
Section 6. Trademarks. is deleted and replaced with:

     6. Trademarks. This License does not grant permission to use the trade
        names, trademarks, service marks, or product names of the Licensor
        and its affiliates, except as required to comply with Section 4(c) of
        the License and to reproduce the content of the NOTICE file.

You may obtain a copy of the Apache License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the Apache License with the above modification is
distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied. See the Apache License for the specific
language governing permissions and limitations under the Apache License.
```
