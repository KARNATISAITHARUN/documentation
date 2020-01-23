# How to Analyze Go Applications Using Inspect

This article shows you how to analyze your applications that are written in Go. It assumes that you have already [installed and authenticated](/inspect/getting-started/README.md) with ShiftLeft.

## Requirements

Inspect analyzes only source code written in Go 1.12 (or later), *not* compiled applications.

## Analyzing Your Go Application

To analyze your Go application, run:

```bash
sl analyze --app <name> --go [<path>]
```

| Parameter | Description |
| - | - |
| `--app <name>` | The name of the application to be analyzed |
| `--go` | The flag identifying the application's language |
| `<path>` | The location of the application's `.go` file to be analyzed (this is the same `.go` file you would pass to `go build`) |
