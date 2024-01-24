---
tags: [software]
---

Without a trailing slash on `src`:
```bash
# Make a copy of src inside of dst [remove extra files in dst/src]
rsync -a src dst [--delete]
```

With a trailing slash on `src`:
```bash
# Copy the contents of src into dst [remove extra files in dst]
rsync -a src/ dst [--delete]
```

Notes:
- `dst` doesn't need to exist, but intermediate directories do
- A trailing slash on `dst` has no effect

Some important flags:
```bash
# Equals -rlptgoD (recursive, include symlinks, preserve permissions and timestamps)
-a, --archive

# Compress during transfer, useful when transferring over a network
-z, --compress

# Print each file that is transferred (redundant with --progress), plus a summary
-v, --verbose

# Show the progress of each file transfer
--progress

# Delete extra files from dest dirs - be careful!
--delete
```

#### Examples
Initial state:
```bash
$ tree
.
├── [ 144]  bar
│   └── [   0]  buzz
└── [ 144]  foo
    └── [   0]  fizz

2 directories, 2 files
```

Without a trailing slash:
```bash
$ rsync -a foo bar
$ tree
.
├── [ 144]  foo
│   └── [   0]  fizz
└── [ 232]  bar
    ├── [   0]  buzz
    └── [ 144]  foo
        └── [   0]  fizz

3 directories, 3 files
```

With a trailing slash:
```bash
$ rsync -a foo/ bar
$ tree
.
├── [ 144]  foo
│   └── [   0]  fizz
└── [ 240]  bar
    ├── [   0]  buzz
    └── [   0]  fizz

2 directories, 3 files
```

With a trailing slash and `--delete`:
```bash
$ rsync -a foo/ bar --delete
$ tree
.
├── [ 144]  foo
│   └── [   0]  fizz
└── [ 144]  bar
    └── [   0]  fizz

2 directories, 2 files
```
