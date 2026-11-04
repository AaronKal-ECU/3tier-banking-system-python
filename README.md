# 3-Tier Banking System

A distributed banking app split into three networked tiers (command-line client, application server, database server) that talk to each other through Pyro5 remote objects.

## What I built
- Three separately runnable tiers registered with a Pyro5 name server, so each one can run on its own machine. The client never touches the database directly.
- PayID-style transfers: look up a recipient by Australian mobile number, confirm their masked name ("Aaron K."), then send.
- Safe money handling: integer cents throughout, tiered transfer fees with caps, idempotency keys so a retried transfer can't go through twice, and settlement in a single SQLite transaction that rolls back on failure.
- Login sessions with a one-hour expiry, transfer status and history, an audit log of logins and transfers, and an export tool that dumps every table to CSV and Excel.

## Tools & tech
Python 3, Pyro5, SQLite, openpyxl.

## What I learnt
- Separating presentation, business logic and data into independent tiers with clear interfaces.
- Remote method calls and service discovery through a name server.
- Why distributed money transfers need idempotency and atomic settlement.
- Evolving a design: `phase1/` holds the earlier two-tier version that kept its state in a JSON file, before the data moved to its own SQLite server.

## Run it
```
pip install -r requirements.txt
```
Then open four terminals in the project folder and start each tier in order:
```
python -m Pyro5.nameserver
python bdb_server.py
python bas_server.py
python bc_client.py
```
Log in with `aaron` / `aaron123`. Run `python export_db.py` afterwards to export the database. To run the tiers on separate machines, see [RUNNING.md](RUNNING.md).

Built for CSI3344 Distributed Systems at Edith Cowan University, 2026.
