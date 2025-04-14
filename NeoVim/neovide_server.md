---
id: neovide_server
aliases: []
tags: []
---

# neovide_server

## 2025/04/10
- Now neovide on Ubuntu server can be run much more conveniently.

---
```bash nv_server.sh
#!/bin/zsh

# set variables
SERVER_USER="sehyun"
SERVER_IP="166.104.44.22"

# SSH
ssh -L 6666:localhost:6666 $SERVER_USER@$SERVER_IP <<'EOF' &
SSH_PID=$!
source ~/.zshrc
zsh
echo "connected to server"
echo "current time: $(date)"
echo "current directory: $(pwd)"

nvim --headless --listen 127.0.0.1:6666

echo ">>> NeoVim in server now running <<<"
EOF

echo ">>> script execution completed <<<"

sleep 2

neovide --server 0.0.0.0:6666

echo ">>> neovide closed, terminating SSH connection <<<"
kill $SSH_PID 2>/dev/null```
---

- With alias in `~/.zshrc`, the script can be run with command `nvserv`.
