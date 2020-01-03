# Iterator

## Fibonacciの実装

https://dev.to/dandyvica/yarit-yet-another-rust-iterators-tutorial-46dk?signin=true

```rust
#[derive(Clone)]
struct Fibonacci {
    fib_n_1: u64,
    fib_n: u64,
    limit: Option<u64>,     // maximum Fibonacci number, None if infinite
}

// Fibonacci sequence is well known
impl Fibonacci {
    fn new(limit: Option<u64>) -> Fibonacci {
        Fibonacci {
            fib_n_1: 0,
            fib_n: 1,
            limit: limit, 
        }
    }
}

// only implements Iterator and not IntoIterator
impl Iterator for Fibonacci {
    type Item = u64;

    fn next(&mut self) -> Option<Self::Item> {
        let next_fib = self.fib_n + self.fib_n_1;

        self.fib_n_1 = self.fib_n;
        self.fib_n = next_fib;

        // infinite sequence?
        if self.limit.is_none() {
            Some(next_fib)             
        }
        else {
            // don't overflow the limit
            if next_fib > self.limit.unwrap() {
                None
            }
            // ok we can loop
            else {
                Some(next_fib)  
            }
        }
    }   
}
```
 
