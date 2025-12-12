# StableCoin_ArbitrageV2
- To install everything you must type in terminal pip install -r requirements.txt
- To run the ui you must type in terminal streamlit run ui.py

## Project Structure & File Descriptions

### ui.py
Streamlit-based user interface for the project.
- Renders the arbitrage graph visually using NetworkX and Matplotlib.
- Allows the user to select starting wallet, available capital, and heuristic.
- Runs the A* arbitrage search and displays the most profitable route.
- Shows a step-by-step execution plan including trades, transfers, fees, and blockchain used.

---

### graph.py
Core graph construction logic.
- Builds a directed graph where nodes represent `(exchange, stablecoin)` wallets.
- Adds **trade edges** (intra-exchange swaps) with taker fees and prices.
- Adds **transfer edges** (cross-exchange withdrawals) with chain, fee, and transfer time.
- Encodes all edge costs as `-log(rate)` so profits compose additively for shortest-path search.

---

### astar_volat.py
A* search implementation for finding the most profitable arbitrage path.
- Searches the graph starting from a selected wallet node.
- Uses accumulated log-cost `g(n)` to model fees and losses.
- Uses a liquidity-based heuristic `h(n)` to avoid illiquid routes.
- Returns the best path, final cash value, and profit.
- Supports pruning via depth and time constraints.

---

### heuristic_volat.py
Liquidity / volume-based heuristic used by A*.
- Fetches 24h trading volume using CCXT.
- Converts volume into an estimated per-second trading flow.
- Compares order size to expected flow within the remaining time window.
- Produces a heuristic penalty that discourages illiquid markets.
- Makes the search more realistic by accounting for execution feasibility.

---

### data.py
Exchange and market configuration.
- Initializes CCXT exchange clients.
- Defines which stablecoins are available on each exchange.
- Maps stablecoins to their corresponding trading pairs.
- Acts as the single source of truth for exchange metadata.

---

### fees.py
Centralized fee configuration.
- Stores taker trading fees per exchange.
- Stores withdrawal fees per exchange, coin, and blockchain.
- Used by `graph.py` to correctly price trade and transfer edges.

---

### transfer_time.py
Blockchain transfer time modeling.
- Stores estimated transfer times for different blockchains (e.g., SOL, ERC20, TRC20).
- Used to model time constraints during arbitrage execution.

---

### show_graph.py
Standalone visualization utility.
- Builds and displays the arbitrage graph without running the UI.
- Useful for debugging graph structure and edge connectivity.

---

### requirements.txt
Python dependencies required to run the project.


