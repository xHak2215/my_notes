
https://habr.com/ru/companies/T1Holding/articles/746860/

```Cargo.toml
[dependencies]
serde_json = {version="1.0.99"}
```

```rust
use std::fs;
use serde_json;


fn main() {
    let res: Result<String, std::io::Error> = fs::read_to_string("data.json"); // read file into string
    let s = match res { // get value from Result object
        Ok(s) => s,
        Err(_) => panic!("Can't read file")
    };

    let mut json_data: serde_json::Value = serde_json::from_str(&s)
        .expect("Can't parse json"); // convert string to json object and panic in case of error

    println!("Data: {}", json_data);
    println!("Bob age: {}", json_data[0]["age"]);

    // change values
    json_data[0]["age"] = serde_json::json!(123);
    json_data[0]["name"] = serde_json::json!("mascai");
    println!("Data: {}", json_data);
    
    // read new data
    std::fs::write("output.json", serde_json::to_string_pretty(&json_data).unwrap())
        .expect("Can't write to file");
}
```
