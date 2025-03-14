# unhide

A Go-based wrapper for Arjun, designed to discover and format hidden HTTP parameters massively.

## Overview

Unhide is a tool that leverages Arjun's parameter discovery capabilities with an improved workflow for web application testing. It discovers hidden parameters in web applications and formats them for easy integration with other security testing tools massively.

## Features

- Process single URLs or multiple URLs from files or stdin
- Multi-threaded processing for faster operations
- Custom parameter wordlists support
- Simplified output format: parameters are presented as `?param1=FUZZ&param2=FUZZ&...`
- Compatible with URL fuzzing tools like ffuf, wfuzz, etc.
- All core Arjun features accessible through a simplified interface

## Requirements

- Go 1.16 or higher
- [Arjun](https://github.com/s0md3v/Arjun) installed and available in your PATH

## Installation

### Install unhide

```bash
go install github.com/gilsgil/unhide@latest
```

### Install Arjun (if not already installed)

```bash
pip3 install arjun
```

## Usage

### Basic Usage

```bash
# Test a single URL
unhide -u https://example.com/path

# Test multiple URLs from a file
unhide -l urls.txt

# Process URLs from stdin
cat urls.txt | unhide
```

### Advanced Options

```bash
# Use custom wordlist
unhide -u https://example.com/path -w /path/to/wordlist.txt

# Specify custom parameters to test
unhide -u https://example.com/path -p "id,page,user,token"

# Add custom headers
unhide -u https://example.com/path --headers "Cookie: auth=123\nUser-Agent: Mozilla/5.0"

# Use more threads for faster processing
unhide -u https://example.com/path -t 10

# Test POST parameters
unhide -u https://example.com/path -m POST

# Enable passive parameter discovery
unhide -u https://example.com/path --passive

# Prefer stability over speed
unhide -u https://example.com/path --stable
```

## Output Format

The tool outputs discovered parameters as URL query strings with FUZZ placeholders:

```
https://example.com/path?id=FUZZ&token=FUZZ&redirect=FUZZ
```

This format can be directly used with fuzzing tools like ffuf:

```bash
unhide -u https://example.com/path | xargs -I{} ffuf -u {} -w payloads.txt
```

## Available Options

| Flag | Description |
| ---- | ----------- |
| `-u` | Single URL to test |
| `-l` | File containing URLs (one per line) |
| `-t` | Number of threads for parallel processing (default: 5) |
| `-w` | Custom wordlist file path to pass to Arjun |
| `-p` | Comma-separated parameters to create a temporary wordlist |
| `-m` | Request method: GET/POST/XML/JSON (default: GET) |
| `--headers` | Additional headers (separate multiple with '\n') |
| `--passive` | Enable passive parameter collection |
| `--stable` | Prefer stability over speed |

## License

This project is licensed under the MIT License - see the LICENSE file for details.
