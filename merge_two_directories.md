# Merge two directories

## Rsync Command

rysnc synchronizes in one direction from source to destination. 

```bash
$ rsync -avh --progress source/ destination/
```

- **-a**: means "archive" and copies everything recursively from source to destination preserving nearly everythin.
- **-v**: gives more output ("verbose")
- **-h**: for human readable
- **--progress**: to show how much work is done

If you want to update the destination files with newer files from source files.

```bash
$ rsync -avhu --progress source/ destination/
```

- **-u**: skip the files that are newer on the receiver
