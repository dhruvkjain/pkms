[[Rust/index|Rust/index]]

tokio has a worker thread per core,
concurrency -> same thread doing multiple task (context switching when one process is doing io etc)
parallelism -> multiple threads doing multiple task at the same time

## Future (not same as Promise)
in javascript the moment we create an async functions it starts running and gives back a promise when called await till the task is done
in javascript a Promise is eager

while in rust, creating async function doesn't start executing the process rather waits for it to be called
so in rust Future is lazy
so to run a async "Future" we need a runtime = tokio, it converts Future to a "Task"
running tokio::spawn(calling async function) makes it Future to start executing

## Structured Concurrency & Cancellation

`spawn` vs `await` :
```rust
#[tokio::main]
async fn main() {
	// fire and forget task (this creates a new task)
	tokio::spawn(logging());
}
```

```rust
#[tokio::main]
async fn main() {
	// in same task 
	// we wait for future to complete
	let user = fetch_user().await;
	// now user can be used as we know for sure that it has completed
}
```


`join!` vs `await`:
`join!` -> better
```rust
#[tokio::main]
async fn main() {
	// in same task
	// we wait for both futures to complete
	// but this is different from writing sequential awaits
	// as join! does interleaving, i.e. if one future is doing io
	// run the second one 
	let (config, db) = tokio::join!(
		load_config(),
		connet_to_db(),
	);
	let user = fetch_user().await;
}
```

```rust
#[tokio::main]
async fn main() {
	let config = load_config().await;
	// wait for config future to fully finish
	let user = fetch_user().await;
	// wait for db future to fully finish
	let user = fetch_user().await;
}
```


# tokio runtime

![[Pasted image 20260912203937.png]]

```rust
#[tokio::main]
async fn main() {
	let user = fetch_user().await();
}
```

 - **Task**  =  tokio's unit of work
 - **Scheduler and Executor**  =  picks a task (scheduler decides) and run it on a thread (has local queues or ring buffers = hot in cache so fast and have no locks, global queue absorb overflow of task)
	 - it allows work stealing if a thread is idle (i.e. both local and global queues are empty)
- **IO Driver / Reactor** (mio api)  =  Event driven, passes task to OS

