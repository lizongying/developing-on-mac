# developing-on-mac
mac上，为了便捷开发，一些配置和工具

## github

### build github-key
~~~
ssh-keygen -t rsa -C "lizongying@msn.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/lizongying/.ssh/id_rsa): /Users/lizongying/.ssh/id_rsa_github
~~~

### github->Settings->Deploy keys->Add deploy key
Title->C02YV25KLVDG
Key->
~~~
cat /Users/lizongying/.ssh/id_rsa_github.pub
~~~
~~~
vim ~/.ssh/config
~~~
```
Host github.com
        HostName github.com
        User git
        IdentityFile /Users/lizongying/.ssh/id_rsa_github
```

### git
If program not exists
~~~
git clone https://github.com/lizongying/developing-on-mac.git
git config user.name lizongying
git config user.email lizongying@msn.com
git branch --set-upstream-to=origin/master master
~~~
If program exists
```
git init
git config user.name lizongying
git config user.email lizongying@msn.com
git add -A .
git commit -m'init'
git push --set-upstream origin master
```

## github program

### build github-key
~~~
ssh-keygen -t rsa -C "lizongying@msn.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/lizongying/.ssh/id_rsa): /Users/lizongying/.ssh/id_rsa_github_developing-on-mac
~~~

### github->Settings->Deploy keys->Add deploy key
Title->C02YV25KLVDG
Key->
~~~
cat /Users/lizongying/.ssh/id_rsa_github.pub
~~~
~~~
cat /Users/lizongying/.ssh/id_rsa_github_developing-on-mac.pub
~~~
Allow write access->yes
~~~
vim ~/.ssh/config
~~~
```
Host github.com.developing-on-mac
        HostName github.com
        User git
        IdentityFile /Users/lizongying/.ssh/id_rsa_github_developing-on-mac
```

### git
If program not exists
~~~
git clone https://github.com/lizongying/developing-on-mac.git
git config user.name lizongying
git config user.email lizongying@msn.com
git remote rm origin
git remote add origin git@github.com.developing-on-mac:lizongying/developing-on-mac.git
git branch --set-upstream-to=origin/master master
git add -A .
git commit -m'init'
git push
~~~
If program exists
```
git init
git config user.name lizongying
git config user.email lizongying@msn.com
git remote add origin git@github.com.developing-on-mac:lizongying/developing-on-mac.git
git add -A .
git commit -m'init'
git push --set-upstream origin master
```

## deploy
### server
```
Host server
        HostName 127.0.0.1
        Port 22
        User root
        IdentityFile /Users/lizongying/.ssh/id_rsa_server
```
### login
```
ssh server
```
```
mkdir /app
```
### rsync
~~~
vim  ~/.bash_profile
~~~
~~~
alias rsync_developing-on-mac='rsync -avz --progress --delete --exclude ".git" --exclude ".gitignore" --exclude ".idea" --exclude ".DS_Store" /Users/lizongying/IdeaProjects/developing-on-mac server:～'
~~~
### autoload bash_profile
~~~
vim ~/.zshrc
~~~
~~~
source ~/.bash_profile
~~~

## admin mongo
~~~
git clone https://github.com/mrvautin/adminMongo.git && cd adminMongo
npm install
npm start or node app
~~~
~~~
{
    "app": {
        "host": "127.0.0.1",
        "port": 4321,
        "password": "1234",
        "locale": "en",
        "context": "dbApp",
        "monitoring": false
    }
}
~~~

## minio
~~~
wget https://dl.min.io/server/minio/release/linux-amd64/minio
chmod +x minio
./minio server /app/data
server {
    listen 80;
    gzip on;
    server_name ;
    location / {
      proxy_pass http://127.0.0.1:9000;
      proxy_set_header Host $host;
    }
}
~~~

## shadowsocks
```
sslocal -s  -p  -l  -k  -m aes-256-cfb -b 0.0.0.0
```
nginx
```
stream {
    upstream shadowsocks {
        hash $remote_addr consistent;
        server 127.0.0.1:1080;
    }
    server {
        listen 2080;
        proxy_pass shadowsocks;
    }
}
```
