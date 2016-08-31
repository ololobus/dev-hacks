Dev hacks
=========

## Content
### [*nix zone](https://github.com/ololobus/dev-hacks/blob/master/nix-zone.md)


#### Hardware temps/fan speed for Mac OS X

https://github.com/Chris911/iStats

```shell
gem install iStats
istats
```

------------------------------


## Git

#### Rebase

`git rerere` -- https://git-scm.com/docs/git-rerere

#### Show stats since selected commit

```
git diff --stat ef01f745d9a6dd14fc9f9941ef8546d078b75ffe
```

------------------------------


## Python

#### Ubuntu pyenv setup

```shell
sudo apt-get install git python-pip make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev
sudo pip install virtualenvwrapper

git clone https://github.com/yyuu/pyenv.git ~/.pyenv
git clone https://github.com/yyuu/pyenv-virtualenvwrapper.git ~/.pyenv/plugins/pyenv-virtualenvwrapper

echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc
echo 'pyenv virtualenvwrapper' >> ~/.bashrc

exec $SHELL
```

#### Kill Python Multiprocessing Pool
SIGQUIT (`Ctrl + \`) will kill all processes even under Python 2.x.


------------------------------


## Ruby/Rails gem/bundler

#### Resolve openssl build issue on Mac OS X 10.11.*

`bundle config build.eventmachine --with-cppflags=-I/usr/local/opt/openssl/include`


------------------------------


## DBA

### PostgreSQL

#### Count, preview and delete duplicates
```sql
SELECT count(id)
FROM (SELECT id, ROW_NUMBER() OVER (partition BY text1, text2 ORDER BY id) AS rnum FROM tus) t
WHERE t.rnum > 1;

SELECT id, text1
FROM (SELECT id, text1, ROW_NUMBER() OVER (partition BY text1, text2 ORDER BY id) AS rnum FROM tus) t
WHERE t.rnum > 1 limit 100;

DELETE FROM tus
WHERE id IN (
    SELECT id
    FROM (SELECT id, ROW_NUMBER() OVER (partition BY text1, text2 ORDER BY id) AS rnum FROM tus) t
    WHERE t.rnum > 1);
```

#### Indexes and tables size

```sql
SELECT relname, relpages
FROM pg_class
ORDER BY relpages DESC;
```

```sql
SELECT relname, (relpages * 8) / 1024 AS size_mb
FROM pg_class ORDER BY relpages DESC LIMIT 10;
```

#### Activity

```sql
select * from pg_stat_activity;
```
