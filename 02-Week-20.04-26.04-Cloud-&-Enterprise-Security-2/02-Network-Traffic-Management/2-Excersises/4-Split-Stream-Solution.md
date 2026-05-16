# 🖥️ Split Stream - Configuring Application Gateway with path-based routing

**Course:** Cloud & Enterprise Security 2 - Network Traffic Management | **Date:** 21 April 2026

---

## Task

**Objective:** Configure an Application Gateway so that `/image/` forwards exclusively to `VM images` and `/video/` forwards exclusively to `VM videos`.

**Requirements:**
- Create a VNet with `Subnet-Images`, `Subnet-Videos` and `Subnet-AppGW`.
- Deploy two Linux VMs with `nginx` and different test pages.
- Configure an Application Gateway with a URL Path Map for path-based routing.

---

## Solution

### Deployment and Configuration

```text
1. Create a VNet with three separate subnets:
   - Subnet-Images
   - Subnet-Videos
   - Subnet-AppGW
2. Create two Linux VMs:
   - VM-Images in Subnet-Images
   - VM-Videos in Subnet-Videos
3. Install nginx on both VMs and customise the home pages:
   - VM Images: "Serving IMAGES"
   - VM Videos: "Serving VIDEOS"
4. Deploy an Application Gateway in the dedicated AppGW subnet.
5. Define two backend pools:
   - Pool-Images
   - Pool-Videos
6. Create two appropriate HTTP settings and health probes.
7. Configure a URL path map:
   - /image/* -> Pool-Images
   - /video/* -> Pool-Videos
8. Connect the listener and routing rule to the path map.
9. Browser test using:
   - http://<App-GW-IP>/image/
   - http://<App-GW-IP>/video/
```

---

## Analysis

| Component | Purpose | Expected result | ✓ |
|----------|-------|------ ---------------|---|
| Separate subnets | Clearly separate roles and data paths | Images, videos and gateway are segmented | ✅ |
| Different nginx homepages | Make backend destination clearly visible | Each VM delivers its specific test response | ✅ |
| Two backend pools | Logically separate path targets | Each path can be specifically assigned to a pool | ✅ |
| URL Path Map | Set up Layer 7 routing | `/image/` and `/video/` are routed to different backends | ✅ |
| Browser test | Verify routing | The respective path displays the correct response | ✅ |

---

## Notes

- **Path-based routing:** Application Gateway can select different backend pools based on the URL path.
- **Dedicated subnet:** An Application Gateway requires its own subnet.
- **Layer 7:** Routing takes place at the HTTP/HTTPS level, not just based on IP and port.

