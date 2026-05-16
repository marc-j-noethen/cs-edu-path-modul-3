# 🖥️ Delicate Balance - Deploying and testing an internal load balancer in the portal

**Course:** Cloud & Enterprise Security 2 - Network Traffic Management | **Date:** 21 April 2026

---

## Task

**Objective:** Deploy an internal Azure Load Balancer in the portal and demonstrate that the website remains accessible after a backend VM fails.

**Requirements:**
- Complete the `quickstart-load-balancer-standard-internal-portal` quickstart.
- Deploy an internal load balancer with two backend VMs.
- Submit a screenshot of the loaded website after stopping one of the backend VMs.

---

## Solution

### Portal steps

```text
1. Follow the official quickstart for an internal standard load balancer in the Azure portal.
2. Create a NAT gateway, VNet, bastion host and the internal load balancer as described in the instructions.
3. Connect two backend VMs to the load balancer’s backend pool.
4. Use a test VM to access the load balancer’s private frontend IP in the browser.
5. Stop one of the two backend VMs.
6. Reload the page in the browser and check that the service is still accessible.
7. Take a screenshot of the successful page load following the failure of a VM.
```

---

## Analysis

| Step | Purpose | Expected result | ✓ |
|---------|-------|---------------------|---|
| Configure internal frontend IP | Make service available internally only | The load balancer is accessible via private IP | ✅ |
| Connect two backend VMs | Create load balancing and redundancy | Both VMs participate in the backend pool | ✅ |
| Open website via test VM | Check load balancer functionality | One of the backend pages loads | ✅ |
| Stop one backend VM | Simulate failover | The service remains accessible via the remaining VM | ✅ |

---

## Notes

- **Internal load balancer:** An internal load balancer distributes traffic only within a private network.
- **Health Probe:** The load balancer automatically removes failed instances from the active distribution.
- **High Availability:** Two backend VMs reduce the risk of a single point of failure.

