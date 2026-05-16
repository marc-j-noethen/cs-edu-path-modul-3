# 🖥️ Traffic Light - Deploy Application Gateway via Azure CLI with the necessary adjustments

**Course:** Cloud & Enterprise Security 2 - Network Traffic Management | **Date:** 21 April 2026

---

## Task

**Objective:** Implement the Application Gateway quickstart via Azure CLI, taking into account the missing adjustments for NSG, region, VM size and app compatibility.

**Requirements:**
- Run the `quick-create-cli` quickstart for Azure Application Gateway.
- Use `germanywestcentral`, VM size `Standard_D2s_v3` and an Express 4.x-compatible application.
- Verify that, after updating, both `myVM1` and `myVM2` are accessible behind the gateway.

---

## Solution

### Important adjustments

```text
1. Adapt all commands from the original documentation to the germanywestcentral region.
2. Create the VMs consistently using the Standard_D2s_v3 size.
3. In the cloud-init or setup script, use Express 4.x instead of Express 5.x so that the sample application runs stably on Ubuntu 22.04.
4. Additionally, provide an NSG configuration:
   - Allow HTTP traffic from the Application Gateway to the backend subnet
   - Do not block required management and health probe communication
5. Then test the gateway and backend pool configuration as described in the Quickstart.
```

### Example of the app fix

```bash
sudo npm install express@4
```

---

## Analysis

| Fix/Step | Purpose | Expected result | ✓ |
|-------------------|-------|----------- ----------|---|
| Change region to `germanywestcentral` | Fulfil task requirement | All resources are created in the required region | ✅ |
| Set VM size to `Standard_D2s_v3` | Consistent and documented provisioning | Both VMs are created consistently | ✅ |
| Use `express@4` instead of `express@5` | Avoid incompatibilities with the sample application | The web app starts up stably | ✅ |
| Correctly add NSG rules | Prevent 502 errors caused by blocked communication | The Application Gateway reaches the backend VMs | ✅ |
| Refresh the browser several times | Check load balancing | `Hello World from host myVM1!` and `Hello World from host myVM2!` appear alternately | ✅ |

---

## Notes

- **Application Gateway:** A Layer 7 load balancer that intelligently forwards HTTP/HTTPS traffic to backends.
- **502 Bad Gateway:** This error often indicates that the gateway does not see the backend application as healthy or does not see it as reachable.
- **Warning ⚠️:** Even minor discrepancies in NSG rules or app versions can result in the gateway being ready but not receiving valid backend responses.

