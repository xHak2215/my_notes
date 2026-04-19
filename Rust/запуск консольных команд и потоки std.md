https://doc.rust-lang.org/std/process/struct.Command.html

```rust
use std::process::Command;

Command::new("ls")
    .arg("-l")
    .arg("-a")
    .spawn()
    .expect("ls command failed to start");
```

перенапровления вывода команды и str потоков
https://stackoverflow.com/questions/21011330/how-do-i-invoke-a-system-command-and-capture-its-output


Command строит и настраивает дочерний процесс. Когда вы создаёте новый экземпляр Command и указываете команду, которую хотите выполнить, любые аргументы, переменные среды и другие настройки, вы определяете конфигурацию для дочернего процесса.


```rust

use std::process::{Command, Stdio};

let system_command = "echo";
let argument = "Hello World";

let echo_hello: std::process::Output = Command::new(system_command)
                        .arg(argument)
                        //.current_dir(TEST_PATH)
                        // Stdio::piped() creates pipes to capture stdout and stderr of a child process which is created by Command. 
                        // we are telling the Command to create a pipe to capture the standard output (stdout) of the child process.
                        .stdout(Stdio::piped())
                        // creating a pipe to capture the standard error (stderr) of the child process.
                        .stderr(Stdio::piped())
                        // output() executes the child process synchronously and captures its output. 
                        //It returns a std::process::Output struct containing information about the process's exit status, stdout, and stderr.
                        .output()
                        .expect("should be able to execute `echo`");
```