# IntoIterator 

```rust
// loop through a vector
let v = vec![0,1,2,3];

// standard loop
for i in v {
    // blah blah
}

// this desugars as
let mut iter = IntoIterator::into_iter(v);
loop {
    match iter.next() {
        Some(i) => { 
            // blah blah 
        },
        None => break,
    }
}
```


## IntoIterator

https://dev.to/dandyvica/yarit-yet-another-rust-iterators-tutorial-46dk

```rust
// a structure holding &str, just a wrapper on &str vector
struct Words<'a> {
    words: Vec<&'a str>,
}

// just split input text into invidual words
impl<'a> Words<'a> {
    fn new(init: &str) -> Words {
        Words {
            words: init.split(" ").collect(),
        }
    }
}

// structure helper for consuming iterator.
struct IntoIteratorHelper<'a> {
    iter: ::std::vec::IntoIter<&'a str>,
}

// implement the IntoIterator trait for a consuming iterator. Iteration will
// consume the Words structure 
impl<'a> IntoIterator for Words<'a> {
    type Item = &'a str;
    type IntoIter = IntoIteratorHelper<'a>;

    // note that into_iter() is consuming self
    fn into_iter(self) -> Self::IntoIter {
        IntoIteratorHelper {
            iter: self.words.into_iter(),
        }
    }
}

// now, implements Iterator trait for the helper struct, to be used by adapters
impl<'a> Iterator for IntoIteratorHelper<'a> {
    type Item = &'a str;

    // just return the str reference
    fn next(&mut self) -> Option<Self::Item> {
            self.iter.next()
    }
}

// structure helper for non-consuming iterator.
struct IterHelper<'a> {
    iter: ::std::slice::Iter<'a, &'a str>,
}

// implement the IntoIterator trait for a non-consuming iterator. Iteration will
// borrow the Words structure 
impl<'a> IntoIterator for &'a Words<'a> {
    type Item = &'a &'a str;
    type IntoIter = IterHelper<'a>;

    // note that into_iter() is consuming self
    fn into_iter(self) -> Self::IntoIter {
        IterHelper {
            iter: self.words.iter(),
        }
    }
}

// now, implements Iterator trait for the helper struct, to be used by adapters
impl<'a> Iterator for IterHelper<'a> {
    type Item = &'a &'a str;

    // just return the str reference
    fn next(&mut self) -> Option<Self::Item> {
            self.iter.next()
    }
}

// structure helper for mutable non-consuming iterator.
struct IterMutHelper<'a> {
    iter: ::std::slice::IterMut<'a, &'a str>,
}

// implement the IntoIterator trait for a mutable non-consuming iterator. Iteration will
// mutably borrow the Words structure 
impl<'a> IntoIterator for &'a mut Words<'a> {
    type Item = &'a mut &'a str;
    type IntoIter = IterMutHelper<'a>;

    // note that into_iter() is consuming self
    fn into_iter(self) -> Self::IntoIter {
        IterMutHelper {
            iter: self.words.iter_mut(),
        }
    }
}

// now, implements Iterator trait for the helper struct, to be used by adapters
impl<'a> Iterator for IterMutHelper<'a> {
    type Item = &'a mut &'a str;

    // just return the str reference
    fn next(&mut self) -> Option<Self::Item> {
            self.iter.next()
    }
}

use std::iter::FromIterator;

// implement FromIterator
impl<'a> FromIterator<&'a str> for Words<'a> {
    fn from_iter<T>(iter: T) -> Self
    where
        T: IntoIterator<Item = &'a str> {

        // create and return Words structure
        Words {
            words: iter.into_iter().collect(),
        }
    }
}

// just split input text into invidual words
impl<'a> Words<'a> {
    fn iter(&'a self) -> IterHelper<'a> {
        self.into_iter()
    }

    fn iter_mut(&'a mut self) -> IterMutHelper<'a> {
        self.into_iter()
    }    
}

let mut uw = Words::new("Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.");

assert_eq!((&uw).into_iter().count(), 19);

// loop through &uw
for w in &uw {
    println!("{}", w);
}

// user iter()
let upper: Vec<_> = uw.iter().map(|w| w.to_uppercase()).collect();
assert_eq!(upper[0], "LOREM");

// we can modify elements now
for w in &mut uw {
    println!("{}", w);
}

// FromIterator tryout
let lipsum_short: Words = vec!["Lorem", "ipsum", "dolor", "sit", "amet"].iter().map(|w| *w).collect();
assert_eq!(lipsum_short.words, vec!["Lorem", "ipsum", "dolor", "sit", "amet"]);
```