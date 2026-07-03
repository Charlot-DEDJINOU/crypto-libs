# crypto-libs

Importable crypto libraries for CTF solve scripts.

This repository is meant to be managed as one top-level tool by `ctftool`, while
its own `setup.sh` manages the nested libraries listed in `libs.tsv`.

## Layout

```text
crypto-libs/
├── coppersmith/
│   ├── kionactf/
│   ├── defund/
│   └── cuso/
├── lattice_tools/
├── crypto_attacks/
├── links/
├── libs.tsv
├── setup.sh
└── env.sh        # generated
```

## Setup

From inside this repository:

```bash
./setup.sh
source ./env.sh
```

`setup.sh` is idempotent:

- missing repositories are cloned;
- existing repositories are updated with `git pull --ff-only`;
- useful compatibility symlinks are recreated in `links/`;
- `env.sh` is regenerated;
- libraries marked `sage-pip-editable`, such as `cuso`, are installed into Sage
  when `sage` is available.

To clone without installing Sage packages:

```bash
./setup.sh --no-sage-install
```

To inspect actions without making changes:

```bash
./setup.sh --dry-run
```

## Adding Libraries

Edit `libs.tsv`. Columns are tab-separated:

```text
name    path    url    branch    actions    description
```

The `path` is relative to this repository. Use `-` as branch to clone the
remote's default branch.

Common actions:

```text
pythonpath
sage-pip-editable
patch:no-msolve
symlink:<link-name>:<target-path>
```

## System Dependencies

This repository does not build heavyweight system tools. Keep these installed
and documented outside the repo:

```text
sage
fplll
flatter
msolve
```

For kionactf/coppersmith, `env.sh` defaults to:

```bash
COPPERSMITHFPLLLPATH=/usr/bin
COPPERSMITHFLATTERPATH=/usr/local/bin
```

Override them before sourcing `env.sh` if your machine uses different paths.
