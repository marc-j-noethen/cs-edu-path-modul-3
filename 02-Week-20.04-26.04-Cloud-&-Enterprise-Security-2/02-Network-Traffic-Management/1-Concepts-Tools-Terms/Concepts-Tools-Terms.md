# Network Traffic Management

## 📊 Summary based on the 80/20 principle

### 1. The key difference is Layer 4 versus Layer 7
The essence of the 80/20 principle is this: an Azure Load Balancer operates at Layer 4 and distributes packets without understanding HTTP. An Application Gateway operates at Layer 7, recognises URLs and headers, and can control web traffic more intelligently.

### 2. Step-by-step core process
1. Determine whether you need to simply distribute load or also understand HTTP content.
2. Choose a Load Balancer for transport traffic and an Application Gateway for web traffic.
3. Use health probes so that failed instances are automatically removed from the traffic.
4. Use Network Watcher to specifically check for reachability and routing issues.

### 3. Interactive mode / Tool usage
For troubleshooting, checking is often the quickest way, rather than creating. Network Watcher shows you immediately whether a connection is working from the platform’s perspective.

### 4. Key concepts with code examples
- **Layer 4:** Recognises ports and protocols such as TCP or UDP, but not the HTTP content.
- **Layer 7:** Understands URL paths, headers and web application logic.
- **Application Gateway:** Can handle web routing, TLS termination and often WAF functions as well.
- **Network Watcher:** Diagnostic tool for connectivity and network paths.

```bash
az network watcher test-connectivity \
  --resource-group rg-net \
  --source-resource vm-app-01 \
  --dest-address 10.1.2.4 \
  --dest-port 443
```

### 5. Comparison: Load Balancer vs. Application Gateway
- Load Balancer distributes L4 traffic quickly and efficiently based on ports and health probes.
- Application Gateway processes L7 web traffic and understands hostnames, URLs and HTTPS.
- For pure web applications, Application Gateway is often the more secure and flexible front door.

### 6. Why is this important / Benefits
Without traffic management, there is neither clean scaling nor high availability. At the same time, these services are central control points for security and diagnostics.

**Quick Start Checklist**
- ☐ I can distinguish between L4 and L7.
- ☐ I know when to use a load balancer.
- ☐ I know when an Application Gateway is a better fit.
- ☐ I know the purpose of Azure Network Watcher.

**Key point**
If you know whether your traffic just needs to be distributed or actually understood, you’ll usually find the right Azure service straight away.

---

## Table 1: Tools used
| Tool | Description |
|---|---|
| Azure Load Balancer | Distributes L4 traffic across multiple instances |
| Azure Application Gateway | Intelligently processes L7 web traffic |
| Azure Network Watcher | Diagnoses connections and network issues |
| Health Probe | Checks whether a back-end target is still healthy |

## Table 2: Technical Terms
| Term | Meaning |
|---|---|
| Layer 4 | Transport layer with TCP and UDP |
| Layer 7 | Application layer with HTTP and HTTPS |
| High Availability | High availability despite failures |
| Health Probe | Automatic health check for target systems |

## Table 3: Key terms
| Term | Meaning |
|---|---|
| route | forward |
| probe | check |
| traffic | traffic |
| failover | automatic switchover |


