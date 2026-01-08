# Explore Docker

Creating a mini Docker system in attempt to understand how Docker actually works \
plus learning some GO ;)


# Go commands
| Command                                              | What it does                                    |
| ---------------------------------------------------- | ----------------------------------------------- |
| `go run main.go`                                     | Runs that file immediately                      |
| `go run .`                                           | Runs all Go files in the current folder/package |
| `go build -o mydocker`                               | Compiles your project into a binary `mydocker`  |
| `./mydocker`                                         | Runs the compiled binary                        |
| `GOOS=linux GOARCH=amd64 go build -o mydocker-linux` | Builds Linux binary on macOS                    |
