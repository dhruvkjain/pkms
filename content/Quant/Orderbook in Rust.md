
https://lobsterdata.com/
https://www.kaggle.com/datasets/freemanone/fi2010

an orderbook is a store of **buy(bid)** and **sell(ask)** orders for a specific asset/financial instrument usually organized by **price-time priority**.

inshort: It shows who wants to **buy or sell**, **how much**, and **at what price**.

### Order Book Levels
The different **"levels"** of order book data represent **how much information** is being shown or used.
1. **Level 1 (L1) - Top of the book**
		Best Bid Offer (BBO), 
		best ask ==> minimum ask somebody is willing to accept
		best bid ==> highest someone is willing to pay
2. **Level 2 (L2) – Market Depth**
		Market by Price (MBP),
		Shows multiple prices, their quantity, number of asks/bids
		With L2, you can answer: “How much could I buy without moving the market too much?”
3. **Level 3 (L3) – Full Depth / Raw Orders**
		Market by Order(MBO),
		Shows every individual order.

### Order Matching Algorithms:
- **Price-Time Priority (FIFO)**: maintain a Priority Queue of asks/bids, FIFO as queue works on it. Match best price first. For equal price, oldest order wins.
- **Pro-Rata Matching**: priority to high quantity at best price. Orders at the **same price level** are matched proportionally based on their sizes.
- **Size-Time Priority** (Hybrid model): larger orders which came ahead of others get matched first if prices are equal. Orders are sorted first by size(like prorata), then by time within same size(like fifo).


## Rust details:
`Box<T>` – Single Owner, Heap Allocation
`Rc<T>` – Multiple Owners (Single Thread)
`Arc<T>` – Multiple Owners (Multi-Thread)

If you want **mutability** inside `Rc` or `Arc`, wrap your type:
- `Rc<RefCell<T>>` for single-threaded mutable access
- `Arc<Mutex<T>>` for multi-threaded mutable access



