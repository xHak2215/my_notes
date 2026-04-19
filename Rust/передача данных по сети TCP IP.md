
### TSP сервер
```rust
use std::net::{TcpListener, TcpStream};
use std::io::{Read, Write};
use std::thread;

fn handle_client(mut stream: TcpStream) {
    // Буфер для чтения данных
    let mut buffer = [0; 1024];
    
    // Чтение данных из потока
    match stream.read(&mut buffer) {
        Ok(size) => {
            // Эхо: отправляем те же данные обратно
            if size > 0 {
                println!("Received: {}", String::from_utf8_lossy(&buffer[0..size]));
                if let Err(e) = stream.write(&buffer[0..size]) {
                    eprintln!("Failed to write to stream: {}", e);
                }
            }
        }
        Err(e) => {
            eprintln!("Failed to read from stream: {}", e);
        }
    }
}

fn main() {
    // Создаем TCP-слушатель на локальном адресе и порту 8080
    let listener = TcpListener::bind("127.0.0.1:8080").unwrap();
    println!("Server listening on port 8080");

    // Принимаем входящие соединения
    for stream in listener.incoming() {
        match stream {
            Ok(stream) => {
                // Запускаем новый поток для обработки клиента
                thread::spawn(|| {
                    handle_client(stream);
                });
            }
            Err(e) => {
                eprintln!("Connection failed: {}", e);
            }
        }
    }
}
```

### TCP клиент

```rust
use std::net::TcpStream;
use std::io::{Read, Write};

fn main() {
    // Подключаемся к серверу
    match TcpStream::connect("127.0.0.1:8080") {
        Ok(mut stream) => {
            println!("Connected to server!");
            
            // Отправляем сообщение
            let message = "Hello, server!";
            stream.write(message.as_bytes()).unwrap();
            println!("Sent: {}", message);
            
            // Получаем ответ
            let mut buffer = [0; 1024];
            match stream.read(&mut buffer) {
                Ok(size) => {
                    println!("Received: {}", String::from_utf8_lossy(&buffer[0..size]));
                }
                Err(e) => {
                    eprintln!("Failed to receive data: {}", e);
                }
            }
        }
        Err(e) => {
            eprintln!("Failed to connect: {}", e);
        }
    }
}
```