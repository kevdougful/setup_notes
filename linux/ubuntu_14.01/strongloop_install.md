#### Install Python 2.7

```
sudo apt-get install build-essential checkinstall
sudo apt-get install libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev
cd /usr/src
sudo wget https://www.python.org/ftp/python/2.7.10/Python-2.7.10.tgz
sudo tar xzf Python-2.7.10.tgz
cd Python-2.7.10
sudo ./configure
sudo make altinstall
```

#### Install Node.js 4

```
curl -sL https://deb.nodesource.com/setup_5.x | sudo -E bash -
sudo apt-get install --yes nodejs
```
might need this symlink: `sudo ln -s /usr/bin/nodejs /usr/bin/node`

#### Install StrongLoop

```
sudo chown -R $USER /usr/local/bin
sudo chown -R $USER /usr/local/lib/node_modules
npm install -g strongloop
```
