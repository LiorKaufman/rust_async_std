### Documentation 
1. https://github.com/async-rs/async-std
2. https://book.async.rs/introduction.html 

### How to install async-std
cargo add async-std

### Tasks 

```
async {
    let result = read_file("data.csv").await;
    match result {
        Ok(s) => println!("{}", s),
        Err(e) => println!("Error reading file: {:?}", e)
    }
};
```
This is an async block. Async blocks are necessary to call async functions, and will instruct the compiler to include all the relevant instructions to do so. In Rust, all blocks return a value and async blocks happen to return a value of the kind Future


```
#![allow(unused)]
fn main() {
extern crate async_std;
use async_std::task;
task::spawn(async { });
}
 ```

spawn takes a Future and starts running it on a Task. It returns a JoinHandle. Futures in Rust are sometimes called cold Futures. You need something that starts running them. To run a Future, there may be some additional bookkeeping required, e.g. whether it's running or finished, where it is being placed in memory and what the current state is. This bookkeeping part is abstracted away in a Task.

A Task is similar to a Thread, with some minor differences: it will be scheduled by the program instead of the operating system kernel, and if it encounters a point where it needs to wait, the program itself is responsible for waking it up again. We'll talk a little bit about that later. An async_std task can also have a name and an ID, just like a thread.

For now, it is enough to know that once you have spawned a task, it will continue running in the background. The JoinHandle is itself a future that will finish once the Task has run to conclusion. Much like with threads and the join function, we can now call block_on on the handle to block the program (or the calling thread, to be specific) and wait for it to finish.