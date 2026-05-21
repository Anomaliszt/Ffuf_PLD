# FFUF Basic Guide

This project contains a very small introduction to **ffuf**.


## TASKS

1. What are the two directories you find ?
2. Fuzz the '/blog' directory and find all pages. One of them should contain a flag. What is the flag? (hint: to find pages you might need to search extensions)
3. Use recursion to find more files/directories. One of them should give you a flag. What is the content of the flag?

## What is ffuf?

ffuf is a tool used for web content discovery. It helps find hidden files, directories, and parameters by sending many requests with wordlists.

## Basic idea

You give ffuf:
- a target URL
- a wordlist
- a pattern to test

Then ffuf checks which paths or names exist.

## Example use

```bash
ffuf -u http://example.com/FUZZ -w wordlist.txt
```

## Extension fuzzing example

You can also use ffuf to fuzz file extensions:

```bash
ffuf -u http://example.com/indexFUZZ -w wordlist.txt
```

This will try values from the wordlist with the extensions diffrent extensions like `.php`, `.txt`,`.html`, etc. 

## Recursive fuzzing example

ffuf can also be used recursively to search deeper into discovered directories:

```bash
ffuf -u http://example.com/FUZZ -w wordlist.txt -recursion -recursion-depth 1
```

This makes ffuf continue fuzzing inside paths it finds.

## Common use cases

- directory discovery
- file discovery
- parameter fuzzing
- extension fuzzing
- recursive fuzzing

## Important note

Only use wordlists found in previous directory :D
