# Running across multiple machines

Each tier can run on its own computer on the same local network.

1. Install the requirements on every machine and copy the project folder to each one:
   ```
   pip install -r requirements.txt
   ```
2. On the machine that will host the name server (machine A), find its local IP address and start the name server bound to it:
   ```
   python -m Pyro5.nameserver --host <machine-A-ip>
   ```
3. On every other machine, point Pyro5 at machine A:
   ```
   export PYRO_NS_HOST=<machine-A-ip>          # macOS / Linux
   set PYRO_NS_HOST=<machine-A-ip>             # Windows CMD
   $env:PYRO_NS_HOST="<machine-A-ip>"          # Windows PowerShell
   ```
4. Start one tier per machine, in this order:
   ```
   python bdb_server.py     # machine 2: database server
   python bas_server.py     # machine 3: application server
   python bc_client.py      # machine 4: client, shows the login screen
   ```

To export the database, run `python export_db.py` on the database machine. It writes a `db_export/` folder with a CSV per table and a single `banking_data.xlsx` workbook.

To stop the system, press `Ctrl + C` in each terminal, client first and name server last.
