# brew install reported "Failed to connect to 127.0.0.1 port 1087"

Well, it really confused me when I see this error message.

```text
Cloning into '/usr/local/Homebrew/Library/Taps/ubuntu/homebrew-microk8s'...
fatal: unable to access 'https://github.com/ubuntu/homebrew-microk8s/': Failed to connect to 127.0.0.1 port 1087: Connection refused
```

 `http_proxy` and `https_proxy` are empty. I toke more than 10 minutes to sort out:

I set the http proxy for my git. So if anyone hit the similar issue, have a look at `~/.gitconfig` file first.

