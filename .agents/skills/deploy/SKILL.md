---
name: deploy
description: Build mgsbot and deploy it to the remote server
disable-model-invocation: true
allowed-tools: Bash
---

Deploy mgsbot to the remote gatobot server. Follow these steps exactly:

## 1. Build the Linux binary

```bash
bun run build:linux
```

This creates the `mgsbot` ELF binary for linux-x64.

## 2. Stop the running process on the server

SSH into the server and find/kill the running mgsbot process:

```bash
ssh eliaquin@178.156.187.168 -p 2299 "pkill -f mgsbot || true"
```

## 3. Upload the new binary

```bash
scp -P 2299 mgsbot eliaquin@178.156.187.168:~/gatobot/mgsbot
```

## 4. Start the new binary

```bash
ssh eliaquin@178.156.187.168 -p 2299 "cd ~/gatobot && nohup ./mgsbot > /dev/null 2>&1 &"
```

## 5. Verify it's running

```bash
ssh eliaquin@178.156.187.168 -p 2299 "ps aux | grep mgsbot | grep -v grep"
```

Confirm the process is listed and report the PID to the user.

## 6. Clean up local binary

```bash
rm mgsbot
```
