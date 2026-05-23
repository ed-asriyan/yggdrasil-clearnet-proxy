# Yggdrasil Clearnet Proxy
Lightweight Docker Compose stack that runs an Yggdrasil node bundled with a SOCKS5 proxy (`microsocks`) in a shared network namespace. It acts as an outbound internet (clearnet) exit node for Yggdrasil clients (like [YggMesh](https://github.com/ed-asriyan/YggMesh)).

## One-Line Deployment
Run the stack remotely without cloning the repo. Pass public Yggdrasil peers (separated by space) via the `YGG_PEERS` env variable:

```bash
YGG_PEERS="tcp://1.2.3.4:5678 tcp://5.6.7.8:9012" curl -sSL https://raw.githubusercontent.com/ed-asriyan/yggdrasil-clearnet-proxy/refs/heads/master/docker-compose.yml | YGG_PEERS="tcp://1.2.3.4:5678 tcp://5.6.7.8:9012" docker compose -p yggdrasil-clearnet-proxy -f - up -d
```

You can find list of public peers [here](https://publicpeers.neilalexander.dev/). It's recommended to add just a few addresses from your country

# Post-Deployment
1. Get your Node's Yggdrasil IPv6 address:
   ```bash
   docker exec yggdrasil yggdrasil -address -useconffile /etc/yggdrasil/yggdrasil.conf
   ```
2. Connect you device to Yggdrasil-only network (e.g. [YggMesh](https://github.com/ed-asriyan/YggMesh))
3. Client Connection Details:
   * Host: *Your Yggdrasil IPv6 Address from the command above* 
   * Port: `1080`
   * Protocol: SOCKS5
