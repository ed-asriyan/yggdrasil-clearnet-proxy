# Yggdrasil Clearnet Proxy
Lightweight Docker Compose stack that runs an Yggdrasil node bundled with a SOCKS5 proxy (`microsocks`) in a shared network namespace. It acts as an outbound internet (clearnet) exit node for Yggdrasil clients (like [YggMesh](https://github.com/ed-asriyan/YggMesh)).

## 🚀 Quick Start
### 1. Generate an Yggdrasil Private Key
You need a static private key so your node's IPv6 address remains constant across reboots. Generate one instantly with this one-liner:

```bash
docker run --rm --entrypoint yggdrasil ghcr.io/yggdrasil-network/yggdrasil-go:latest -genconf | grep PrivateKey
```

(Save the output string, you will need it in the next step).

### 2. Run the Mirror
You don't need to clone this repository. Run the setup directly using the raw Compose file:
```bash
YGG_PRIV_KEY="<YOUR_GENERATED_PRIVATE_KEY>" \
YGG_PEERS="tcp://1.2.3.4:12345 tls://5.6.7.8:9012" \
docker compose -f <(curl -fsSL https://raw.githubusercontent.com/ed-asriyan/yggdrasil-clearnet-proxy/master/docker-compose.yml) up -d
```

| Variable | Required | Description |
| -------- | -------- | ----------- |
| `YGG_PRIV_KEY` | Yes | Your Yggdrasil private key. |
| `YGG_PEERS` | Yes | A space-separated list of Yggdrasil peers. *Find active public peers at [Yggdrasil Public Peers](https://publicpeers.neilalexander.dev).* |

### 3. Get your Yggdrasil IPv6 Address
Retrieve the IPv6 address assigned to your container's tun0 interface:
```bash
docker compose -p yggdrasil-clearnet-proxy logs yggdrasil | grep "Your IPv6 address is"
```

### 4. Register in Alfis DNS (optional)
1. Open your [Alfis GUI](https://alfis.name).
2. Mine a new key if you haven't done it before
3. Register your target .ygg domain.
4. Create an AAAA record pointing to the IPv6 address obtained in Step 3.

## Connecting a client
1. Connect you device to Yggdrasil-only network (e.g. [YggMesh](https://github.com/ed-asriyan/YggMesh))
2. Client Connection Details:
   * Host: *Your Yggdrasil IPv6 Address* OR *Alfis domain*
   * Port: `1080`
   * Protocol: SOCKS5
