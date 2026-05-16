# 🖥️ End-to-End – Configuring Application Gateway with end-to-end TLS encryption

**Course:** Cloud & Enterprise Security 2 – Network Traffic Management | **Date:** 21 April 2026

---

## Task

**Objective:** Set up an Azure Application Gateway so that HTTPS remains encrypted end-to-end from the client to the backend servers.

**Requirements:**
- Complete the official PowerShell tutorial for end-to-end TLS with Application Gateway.
- Prepare frontend and backend certificates, as well as genuine HTTPS-enabled backend servers.
- Submit a screenshot of successful HTTPS access, including certificate information.

---

## Solution

### Preparation

```text
1. Install PowerShell 7.x and the Az module.
2. Prepare a front-end certificate in .pfx format, including a password.
3. Run at least two back-end web servers with TLS on port 443.
4. Export the trusted root certificate for the back-end TLS chain as a `.cer` file. In a simple lab with a self-signed backend certificate, this can be the backend certificate itself.
5. Note down the private IP addresses of the back-end servers.
```

### PowerShell steps

```powershell
$password = ConvertTo-SecureString "PfxPasswordHere" -AsPlainText -Force
$frontendCert = New-AzApplicationGatewaySslCertificate -Name "frontend-cert" -CertificateFile "C:\certs\frontend.pfx" -Password $password
$backendTrustedRoot = New-AzApplicationGatewayTrustedRootCertificate -Name "backend-root-cert" -CertificateFile "C:\certs\backend-root.cer"
```

```text
Then, following the tutorial:
1. Create a VNet, subnets and a public front-end IP.
2. Create a backend pool with the actual private IP addresses of the HTTPS servers.
3. Configure the HTTPS listener with the frontend .pfx certificate.
4. Set the backend HTTP settings to HTTPS/443 and, for Application Gateway v2, add the backend trusted root certificate if the backend is using a private or self-signed PKI.
5. Create a routing rule and deploy the gateway.
6. Test the frontend IP in the browser via https:// and view the certificate details.
```

---

## Analysis

| Step | Purpose | Expected result | ✓ |
|---------|-------|---------------------|---|
| Import `.pfx` for frontend | Be able to terminate TLS at the gateway | The gateway has a valid frontend certificate | ✅ |
| Store the backend trusted root certificate | Establish trust in the backend servers | Backend health probes and HTTPS connections work | ✅ |
| Populate the backend pool with real servers | Replace placeholder IPs from the documentation | The gateway can reach real targets | ✅ |
| Configure HTTPS listener and HTTPS backend settings | Enable end-to-end encryption | Traffic is encrypted from client -> gateway -> backend | ✅ |
| Browser test with certificate view | Verify final result | The page loads successfully via HTTPS and certificate details are visible | ✅ |

---

## Notes

- **End-to-End TLS:** Application Gateway decrypts TLS at the frontend and re-encrypts the connection to the backend.
- **Frontend vs. backend certificate:** The `.pfx` is used by the gateway itself, whilst the backend root certificate establishes trust in the backend servers.
- **Modern SKU detail:** Application Gateway v2 uses trusted root certificates for private/self-signed backend PKI. Older tutorials may still show authentication certificates for v1.
- **Warning ⚠️:** Without actual backend servers running HTTPS on port 443, the health probes – and consequently the entire routing – will fail.
