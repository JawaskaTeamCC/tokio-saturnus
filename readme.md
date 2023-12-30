# Tokio Saturnus

An async programming library for ComputerCraft.

Why? Usually you have to "pull" events, which is both
tedious and destructive.

With Tokio you only have to mark your main function (if
you're using Saturnus), or use the `Ev` methods to run
a cool event loop!

Why an event loop?

 - Easy to manage and use
 - Multiple listeners for the same event
 - Async programming
 - Parallel programming

## Examples

Simple multithreading (*Not real threads) and async/await:

```rs
use { ev, tokio } in tokio;

@tokio()
fn main() {
    ev->launch(() => {
        print("This is async!");
        ev->timeout_promise(2)->await();
        print("This happens after 2 seconds!");
    });
    ev->launch(() => {
        print("Woah! in parallel!");
        ev->timeout_promise(3)->await();
        print("This also after 3 in another thread!");
    });
}
```

Use non-blocking event listeners:

```rs
use { ev, tokio } in tokio;

@tokio()
fn main() {
    ev->on("modem_message", () => {
        print("You've got mail!");
    });
    // Event recasting
    ev->on("modem_message", (_, _, _, msg) => {
        if msg.type == "chat" {
            ev->push("chat_message", msg.data);
        }
    });
    // How to handle them:
    ev->on("chat_message", (_, data) => {
        print("You've got chat!", data);
    });
}
```

Why "tokio"? Because the _Rust_ library _"tokio.rs"_ gave
me the idea or the inspiration, but this project resembles
more to the _NodeJS's event loop_ than anything.
