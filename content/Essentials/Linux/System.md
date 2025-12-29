## top
> [!warning] use htop instead

> **top** --> shows running processes, CPU, memory usage, load average, and more.

| Flag | What it does                            | Example           |
| ---- | --------------------------------------- | ----------------- |
| `-n` | Run for 1 iteration then exit           | `top -n 1`        |
| `-b` | Run in batch mode (for scripts/logging) | `top -b -n 1`     |
| `-u` | Show processes for a specific user      | `top -u username` |

| Key         | What it does                                    |
| ----------- | ----------------------------------------------- |
| `q`         | Quit                                            |
| `h`         | Help (list of commands)                         |
| `P`         | Sort by CPU usage (default)                 |
| `M`         | Sort by Memory usage                        |
| `N`         | Sort by PID                                 |
| `T`         | Sort by Running Time                        |
| `k`         | Kill a process (enter PID when prompted)        |
| `r`         | Renice a process (change priority)              |
| `1`         | Toggle showing individual CPU cores             |
| `Shift + f` | Open column field manager to add/remove columns |


### htop
> **htop** --> an improved version of `top` with a colorful, interactive UI, mouse support, easy process tree, and better sorting/filtering.

| Feature            | What it does                              |
| ------------------ | ----------------------------------------- |
| Arrow Keys / Mouse | Navigate the process list                 |
| `F1`               | Help                                      |
| `F2`               | Setup (customize colors, meters, columns) |
| `F3`               | Search for processes                      |
| `F4`               | Filter processes                          |
| `F5`               | Tree view (show process hierarchy)        |
| `F6`               | Sort by column                            |
| `F7`               | Decrease priority (nice -1)               |
| `F8`               | Increase priority (nice +1)               |
| `F9`               | Kill a process                            |
| `F10`              | Quit                                      |
