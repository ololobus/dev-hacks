Dev hacks
=========

#### *nix zone

##### Remove `<CR>` characters from file (Mac OS X tested)

`sed -i.bak $'s/\r//' file`


##### Find large files

```shell
sudo find / -size +500000 -print
```
```shell
sudo find / -size +500000 -exec sudo ls -lah "{}" \;
```


##### Hardware temps/fan speed for Mac OS X

https://github.com/Chris911/iStats

```shell
gem install iStats
istats
```


------------------------------


#### Python

##### Ubuntu pyenv setup

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


------------------------------


#### Ruby/Rails gem/bundler

##### Resolve openssl build issue on Mac OS X 10.11.*

`bundle config build.eventmachine --with-cppflags=-I/usr/local/opt/openssl/include`


------------------------------


#### DBA

##### PostgreSQL
