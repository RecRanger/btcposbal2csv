# btcposbal2csv

## Forked by xwings
Upgraded to python3

## Dump Bitcoin addresses with positive balance

Simple utility to list all bitcoin addresses with positive balance. It works by analysing the current unspent transaction output set (UTXO), aggregating outputs to same addresses together, and writing them to csv file.

### Prerequisites
* python3
* pip

### Installation
Run `pip install -r requirements.txt`

#### Other Option: Manual Installation

Install the following packages:

For Linux：
* plyvel
* base58
* sqlite3

For Windows：
* plyvel-win32
* base58
* pysqlite3

### Usage
To use this script, you will need copy of chainstate database as created by the [Bitcoin Core](https://bitcoin.org/en/bitcoin-core/)
client.

To get current addresses with positive balance, let the full node client sync with the network. 
**Stop** the bitcoin-core client before running this utility. If you not stop the client, the database might get corrupted.  
Then run this program with path to chainstate directory (usually `$HOME/.bitcoin/chainstate`).

Show help:
```bash
python btcposbal2csv.py -h
```

#### Example
The following will read from `/home/USER/.bitcoin/chainstate`, and write result to `/home/USER/addresses_with_balance.csv`.

```bash
python btcposbal2csv.py /home/USER/.bitcoin/chainstate /home/USER/addresses_with_balance.csv
```

#### Notice
* The output may not be complete as there are some transactions which are not understood by the decoding lib, or that which do not have "address" at all. Such transactions are not processed. Number of them and the total ammount in such transactions is displayed after the analysis.  
* The output csv file only reflects the chainstate leveldb at your disk. So it will always be few blocks behind the network as you need to stop the bitcoin-core client.

#### Converting to RIPEMD160
Per request, I'm adding script which is able to convert BTC address to RIPEMD160 representation.
BTC address must be in fist column, RIPEMD160 is added to csv. Output goes to stdout.

Example:
```bash
python convert2ripemd160.py /home/USER/addresses_with_balance.csv
```

#### Acknowledgement
This utility builds on very nice [bitcoin_tools](https://github.com/sr-gi/bitcoin_tools/) lib,
 which does the UTXO parsing.

This utility is a fork of [graymauser's btcposbal2csv](https://github.com/graymauser/btcposbal2csv).
 
#### Support
If you like this utility, please consider supporting the bitcoin_tools library which does all the heavy lifting.


## FAQ
- **Can this be used for coin XYZ?** Currently only BTC is supported. The direct BTC derivatives (LTC, BCH, and others) should be fairly straightforward to add, other not so much.
