
https://doc.rust-lang.ru/book/ch06-02-match.html

```rust
use std::fs::File;

fn main() {
    let greeting_file_result = File::open("file.txt"); // открывам файл

    let greeting_file = match greeting_file_result { 
        Ok(file) => file,
        Err(error) => panic!("Problem opening the file: {error:?}"),
    };
}
```

https://doc.rust-lang.ru/book/ch09-02-recoverable-errors-with-result.html