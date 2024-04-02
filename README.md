# redis_ts

[![crates.io](https://img.shields.io/badge/crates.io-v0.5.4-orange)](https://crates.io/crates/redis_ts)
![Continuous integration](https://github.com/tompro/redis_ts/workflows/Continuous%20integration/badge.svg)

redis_ts provides a small trait with extension functions for the 
[redis](https://docs.rs/redis) crate to allow 
working with redis time series data that can be installed as 
a [redis module](https://oss.redislabs.com/redistimeseries). Time 
series commands are available as synchronous and asynchronous versions.
 
The crate is called `redis_ts` and you can depend on it via cargo. You will 
also need redis in your dependencies. It has been tested against redis 0.25.2 
but should work with versions higher than that.

 ```ini
 [dependencies]
 redis = "0.25.2"
 redis_ts = "0.5.4"
 ```

 Or via git:

 ```ini
 [dependencies.redis_ts]
 git = "https://github.com/tompro/redis_ts.git"
 ```

With async feature inherited from the [redis](https://docs.rs/redis) crate (either: 'async-std-comp' or 'tokio-comp):

```ini
 [dependencies]
 redis = "0.25.2"
 redis_ts = { version = "0.5.4", features = ['tokio-comp'] }
``` 
 
 ## Synchronous usage
 
 To enable redis time series commands you simply load the 
 redis_ts::TsCommands into the scope. All redis time series 
 commands will then be available on your redis connection.
 
  
 ```rust
 use redis::Commands;
 use redis_ts::{TsCommands, TsOptions};
 
 let client = redis::Client::open("redis://127.0.0.1/")?;
 let mut con = client.get_connection()?;
 
 let _:() = con.ts_create("my_ts", TsOptions::default())?;
 ```
 
 ## Asynchronous usage 
 
 To enable redis time series async commands you simply load the 
 redis_ts::TsAsyncCommands into the scope. All redis time series 
 commands will then be available on your async redis connection.
 
 ```rust
 use redis::AsyncCommands;
 use redis_ts::{AsyncTsCommands, TsOptions};
 
let client = redis::Client::open("redis://127.0.0.1/")?;
let mut con = client.get_async_connection().await?;
 
let _:() = con.ts_create("my_ts", TsOptions::default()).await?;
```

## Compatibility note

Versions >= 0.5 contains a breaking change in the argument list of range queries. With some recent additions in the 
Redis time series module the number of arguments for ts_range, ts_revrange, ts_mrange and ts_mrevrange have simply 
grown to long. All existing and the new arguments are now replaced by a single `TsRangeQuery` struct for which there
is also a builder available.
