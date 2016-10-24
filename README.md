export-surround-to-git
======================

Python script to export history from Seapine Surround in a format parsable by `git fast-import`.

This method is capable of preserving complete history, timestamps, authors, comments, branches, snapshots, etc.

# Disclaimer of Fork
The changes here represent an 'acceptable' use case for my organization. Certain functionality
like handling renames and deletes (https://github.com/JElchison/export-surround-to-git/issues/34)
are not handled by these changes. We accepted and were tolerant of limited history as part 
of our migration from Seapine Surround.

I also strongly recommend doing a directory diff. There are a few edge cases where recently
changed files in Surround may not actually be pulled. Generally this only occurs once or twice
per repo (in my cases).

Current usage example for this fork.
Be sure to GET the repo from surround that you're planning on migrating.

```
export-surround-to-git.py parse -m sites -p "repo"
export-surround-to-git.py -m sites -p "repo" export -d database.db | git fast-import
sqlite3 database.db
sqlite3 > UPDATE operations SET origPath = REPLACE(origPath, 'repo', '');
```

# Usage
```
usage: export-surround-to-git.py [-h] [-m MAINLINE] [-p PATH] [-d DATABASE]
                                 [--version]
                                 [command]

Exports history from Seapine Surround in a format parsable by `git fast-import`.

positional arguments:
  command

optional arguments:
  -h, --help            show this help message and exit
  -m MAINLINE, --mainline MAINLINE
                        Mainline branch containing history to export
  -p PATH, --path PATH  Path containing history to export
  -d DATABASE, --database DATABASE
                        Path to local database (only used when resuming an
                        export)
  --version             show program's version number and exit
```

## Example flow
```
sscm setclient ...
git init my-new-repo
cd my-new-repo
export-surround-to-git.py parse -m surround-branch -p "repo"
export-surround-to-git.py -m surround-branch -p "repo" export -d database.db | git fast-import
...
git repack ...
```


# Further Reading

See the [manpage for git fast-import](https://www.kernel.org/pub/software/scm/git/docs/git-fast-import.html).
