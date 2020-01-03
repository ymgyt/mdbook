# Develop 

## Build

cross compile
```bash
cargo install cross
cross build --target x86_64-unknown-linux-musl
```

### target

`{arch}-{vendor}-{sys}-{abi}` for example, x86_64-unknown-linux-musl

* Archtectureは`uname -m`
* Vendorはlinuxならunknown, windowsならpc, OSXはapple
* Systemは`uname -s`
* ABIは`ldd --version`

`rustc --print target-list` で一覧がみれる


## Install

```console
# rustup
curl https://sh.rustup.rs -sSf | sh
```