# Python

### Virtual envs
Setup
```shell
python -m venv ./venv_name
```

Activate
```shell
source venv_name/bin/activate
```

### Manage python packages in a ruby bundler manner
```shell
$ cat requirements.txt
  requests>=2.12.1,<3.0.0
$ pip install -r requirements.txt
$ pip freeze > requirements.lock.txt
$ cat requirements.lock.txt
  requests==2.12.1
```

### Ubuntu pyenv setup

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

### Kill Python Multiprocessing Pool
SIGQUIT (`Ctrl + \`) will kill all processes even under Python 2.x.
