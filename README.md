# macos-arm64-migration-utils

Tools for migrating away from amd64 (x86_64) binaries on your Apple M1/M2/... Mac
towards a world with only native arm64 binaries.

On macOS, you can have 3 kinds of binaries:
 * arm64
 * amd64 (x86_64)
 * universal binary which package both a arm64 and an amd64 binary

For maximum performance you must get rid of amd64 only binaries.

## Diagnosis

### List all amd64 binaries in $PATH

This will give you a general view of the amount of cleanup to do.

```console
$ ./find-x86-bin
````

The cleanup method depend on how you got the binary installed in the first place.

### List all amd64 binaries in /Applications

```console
$ ./find-x86-bin /Applications
````

For those cases, the simplest thing to do is to delete the app and reinstall
the latest version: if the app had only amd64 binaries at the time you had installed
it (maybe 1 year ago), a new release is probably available that provides universal
binaries.


## Cleaning Homebrew

Homebrew for amd64 is installed in /usr/local.
Homebrew for arm64 is installed in /opt/homebrew.

So if you have some traces of Homebrew in /usr/local/bin, /usr/local/Cellar on an arm64
machine, you must clean them as everything should be in /opt/homebrew.

Check those commands:

```console
$ which brew
```
If you get /usr/local/bin/brew, you must uninstall the amd64 Homebrew and install the arm64 one.

```console
$ ls -l /usr/local/bin | grep Cellar
```


### Remove all Homebrew arm64 binaries

Remove all symlinks in /usr/local/bin that point to Homebrew packages in /usr/local/Cellar:

```console
$ ./unlink-Cellar-pkg -a
```

Unlink the binaries of the single Homebrew package `jq`:

```console
$ ./unlink-Cellar-pkg jq
```


