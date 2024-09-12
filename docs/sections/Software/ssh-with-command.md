# How to SSH into server without typing the full command 

## 1. open your console profile 

eg im using Zsh

```bash
nano ~/.zshrc
```
If you are using bash try:

```
nano ~/.bashrc 
```


## 2. Add the ssh command

```bash
alias sshto='expect -c '\''spawn ssh dev@domain.ke; expect "password:"; send "oaosasa\r"; interact'\'
```

here we are are creating an alias for the command ```sshto``` which means each time you type ```sshto``` the terminal will
automatically ssh into the addre you provided.

## 3. Reload bash profile 

```bash
source ~/.zshrc    # For Zsh 

source ~/.bashrc   # For Bash
```

## Note:
The above process is best used only in local testing environments. THIS WILL EXPOSE YOUR CREDENTIALS

Alternatively you can use ssh keys for more secure automation
