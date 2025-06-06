# 📘 Entra Security Monitor — Documentation

Welcome to the official documentation for **Entra Security Monitor**, a lightweight, read-only web platform that provides visibility into your Microsoft Entra ID (formerly Azure AD) security posture. This guide will help you set up and deploy the platform in your environment.

---

## 🚀 Project Overview

Entra Security Monitor gives you real-time insights into:
- Credential hygiene (expired, unused, or overlapping secrets)
- Admin account risks
- Conditional Access policy effectiveness
- Application secret expiry monitoring
- Entra-related user and sign-in anomalies

---

## 📌 Requirements

Before deploying the platform, ensure the following are in place:

### 🔐 Azure App Registration

1. Navigate to **Azure Active Directory → App registrations**
2. Register a new application called: `Entra Security Monitor`
3. Set **Platform Type** to: `Web`
4. Set the **Redirect URI** to:
   ```
   https://<your-webapp-name>.azurewebsites.net
   ```

---

## 🛂 Required Microsoft Graph API Permissions

> The app must be granted **Application** permissions, not Delegated. You will need Global Admin privileges to consent.

| Permission Name          | Type        | Description                              |
|--------------------------|-------------|------------------------------------------|
| `Directory.Read.All`     | Application | View directory data                      |
| `AuditLog.Read.All`      | Application | Read audit log data                      |
| `Policy.Read.All`        | Application | Read all conditional access policies     |
| `App.Read.All`           | Application | View application metadata                |
| `Reports.Read.All`       | Application | View sign-in activity reports            |
| `User.Read.All`          | Application | Read user profiles                       |

---

## ⚙️ Environment Variables (Required)

When deploying the app (via Azure Web App or other), the following environment variables must be set:

| Variable Name              | Purpose                                 |
|----------------------------|-----------------------------------------|
| `TENANT_ID`                | Your Azure AD tenant ID                 |
| `CLIENT_ID`                | The App Registration (client) ID        |
| `CLIENT_SECRET`            | The App secret for authentication       |
| `GRAPH_API_SCOPE`          | Set to `https://graph.microsoft.com/.default` |
| `AUTHORITY`                | e.g. `https://login.microsoftonline.com/<TENANT_ID>` |

---

## 🧪 Post-Deployment

After deploying, verify:
- The app loads via `https://<your-site>.azurewebsites.net`
- All tabs load valid data (no auth errors)
- You have granted Admin Consent in Azure AD under the app's API permissions

---

## 📄 Additional Resources

- [Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Microsoft Entra Documentation](https://learn.microsoft.com/en-us/entra/)
- [Application Registration Docs](https://learn.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app)

---

## 📬 Questions or Contributions?

Submit an issue or pull request via GitHub:  
👉 [https://github.com/thatlazyadmin/entra-security-monitor](https://github.com/thatlazyadmin/entra-security-monitor)

---

_© 2025 Shaun Hardneck | URBANNERD CONSULTING_