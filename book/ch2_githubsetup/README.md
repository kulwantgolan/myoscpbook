# Setup Github

1. git clone the myoscpbook folder
2. Set up `~/.gitconfig file

```
# Setup Github

1. git clone the myoscpbook folder
2. Set up `~/.gitconfig file

```
# Setup Github

1. git clone the myoscpbook folder
2. Set up `~/.gitconfig file
  - Setup User
  - Use SSH instead of https

```
[user]
     email = youremail@company.com
     name = Kulwant Kumar
[url "ssh://git@github.com/"]
     insteadOf = git://github.com/
     insteadOf = https://github.com/
```

3. Setup SSH Key

[github help to setup SSH](https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key)

```
ssh-keygen -t rsa -b 4096 -C "youremail@company.com"
# start the ssh-agent in background
eval $(ssh-agent -s)
ssh-add ~/.ssh/id_rsa

Add ssh public key(id_rsa.pub) in github > Settings > SSH and GPG keys.  
```

4. Add, commit and push
   
```
git add .
git commit -m "Message"
git push origin master
```
