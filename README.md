# ☀️ Vana

![Vana Image](https://i.imgur.com/tvAuEGr.png)

---

## ☀️ Community Links

- [Vana Discord](https://discord.gg/withvana)
- [Vana Twitter](https://x.com/withvana)
- [Vana Website](https://www.vana.org/)
- [Vana Blog](https://www.vana.org/post)
- [Vana github](https://github.com/vana-com)
- [Vana Explorer](https://satori.vanascan.io/)

---

## 💻 System Requirements

| Components  | Minimum Requirements |
|-------------|----------------------|
| CPU         | 2 Cores               |
| RAM         | 4+ GB                 |
| Storage     | 50++ GB SSD            |
| Ubuntu      | 22.04         |



## ☀️ Setup Steps

### 1. Update and Upgrade System Packages

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install git -y

```

### 2. Install Docker

```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y

```

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

```
```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

```

```bash
sudo apt install docker-ce docker-ce-cli containerd.io -y
```

```bash
sudo systemctl status docker

```

```bash
sudo usermod -aG docker ${USER}

```

### 3. Get Faucet from Satori

☀️ **Please consider using a burner wallet for added security.**
 
[Vana Satori Faucet](https://faucet.vana.org/satori)


### 4. Clone the repository


```bash
git clone https://github.com/sixgpt/miner.git vanacgpt

```

**Add your wallet private key**

```bash
export VANA_PRIVATE_KEY=<your_private_key>
export VANA_NETWORK=satori
```

```bash
docker compose up
```


```bash
docker logs -f --tail 100 vanacgpt-sixgpt3-1
```

![logs](https://raw.githubusercontent.com/xhinis/vana/refs/heads/main/vanasixgpt.png)





### ☀️ EXTRA

** [Vana Telegram Miner](https://t.me/VanaDataHeroBot/VanaDataHero?startapp=1387245589) **
** [Vana Web Miner](https://sixgpt.xyz/miner) **

