# 🖥️ Cloud Plumbing - Identifying a blocked NSG rule using IP Flow Verify

**Course:** Cloud & Enterprise Security 2 - Network Traffic Management | **Date:** 21 April 2026

---

## Task

**Objective:** Use `IP flow verify` from Azure Network Watcher to identify which security rule is blocking a network flow.

**Prerequisites:**
- Complete the official quickstart on diagnosing NSG rules.
- Use `IP flow verify` to analyse blocked traffic.
- Submit a screenshot showing `Access: Denied` and the name of the blocking security rule.

---

## Solution

### Portal steps

```text
1. Complete the quickstart for Network Watcher and NSG diagnostics.
2. Search for "Network Watcher" in Azure and open the service.
3. Under the diagnostic tools, select `IP flow verify` or `NSG diagnostics`.
4. Enter the affected VM, direction, protocol, source and destination IP addresses, and port as specified in the task.
5. Start the check.
6. Evaluate the result and note the status `Access: Denied` and the name of the rule responsible.
7. Take a screenshot of the results page.
```

---

## Analysis

| Step | Purpose | Expected result | ✓ |
|---------|-------|---------------------|---|
| Open Network Watcher | Provide diagnostic tool | The analysis interface is available | ✅ |
| Define traffic parameters | Accurately replicate the data flow in question | The test refers to the correct port and direction | ✅ |
| Run `IP flow verify` | Check rule evaluation against NSG | The tool returns `Access: Allowed` or `Access: Denied` | ✅ |
| Check results page | Identify cause | The blocking security rule is displayed by name | ✅ |

---

## Notes

- **IP flow verify:** The tool assesses whether a packet is allowed or denied based on NSG and admin rules.
- **Diagnostic benefit:** In addition to `Denied`, the tool directly displays the responsible rule, which greatly speeds up troubleshooting.
- **NSG rules:** Both subnet- and NIC-bound rules can affect the flow.

