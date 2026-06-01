# mole

A Bash script for tracking and reopening files you've previously edited, with support for groups, date filters, and compressed access logs.

## Requirements

- `MOLE_RC` environment variable must be set to the path of the configuration file (e.g. `export MOLE_RC=~/.config/molerc`)
- A terminal editor available (`$EDITOR`, `$VISUAL`, or `vi` as fallback)

## Usage

```
mole -h
mole [-g GROUP] FILE
mole [-m] [-g GROUP] [-a DATE] [-b DATE] [DIRECTORY]
mole list [-g GROUP] [-a DATE] [-b DATE] [DIRECTORY]
mole secret-log [-a DATE] [-b DATE] [DIRECTORY...]
```

## Commands

### Open a file
```bash
mole [-g GROUP] FILE
```
Opens `FILE` in your editor and records the file path, group, and timestamp in `MOLE_RC`.

### Open most recently (or most frequently) edited file in a directory
```bash
mole [-m] [-g GROUP] [-a DATE] [-b DATE] [DIRECTORY]
```
If a directory is given (or defaults to the current directory), selects a file from the edit history and opens it.

- Without `-m`: opens the **most recently** edited file
- With `-m`: opens the **most frequently** edited file

### List tracked files
```bash
mole list [-g GROUP] [-a DATE] [-b DATE] [DIRECTORY]
```
Prints a formatted list of tracked files in the given directory (or current directory), showing each file name and its associated group(s).

### Generate a secret log
```bash
mole secret-log [-a DATE] [-b DATE] [DIRECTORY...]
```
Creates a compressed (`.bz2`) log file at `~/.mole/log_<user>_<timestamp>.bz2`, containing a sorted record of file paths and their edit timestamps. Accepts multiple directories; defaults to all tracked files if none are given.

## Options

| Option | Description |
|--------|-------------|
| `-h` | Print help and exit |
| `-g GROUP` | Filter or tag with a group name |
| `-m` | Select the most frequently opened file |
| `-a DATE` | Filter entries opened **after** the given date |
| `-b DATE` | Filter entries opened **before** the given date |

## Configuration

All tracking data is stored in the file pointed to by `$MOLE_RC`. Each entry follows this internal format:

```
# /absolute/path/to/file,group1 group2,2024-01-01_10-00-00 2024-01-02_11-30-00
```

The directory is created automatically if it does not exist.
