# ASP.NET Web API with OBO OAuth Flow — Full Conversation Archive

> This file captures the entire conversation between the User and Assistant, including all dialogs and code samples, in Markdown format.

---

## 1) User
**Message:**  
provide asp.net code sample for a web API application that uses OBO OAuth flow

---

## 1) Assistant
Got it 👍 You want an **ASP.NET (Core) Web API** sample that implements the **On-Behalf-Of (OBO) OAuth 2.0 flow**.  

This flow is typically used when:  
- A frontend (e.g., React, SPA, mobile, etc.) obtains a token from Azure AD (Entra ID).  
- That frontend calls your Web API **with a bearer token**.  
- Your Web API needs to call **another downstream API** (e.g., Microsoft Graph, another custom API) on behalf of the signed-in user.  

Below is a **.NET 6/7 minimal Web API example** using **Microsoft.Identity.Web** which simplifies OBO:  

### `Program.cs`
```csharp
using Microsoft.AspNetCore.Authentication.JwtBearer;
using Microsoft.Identity.Web;

var builder = WebApplication.CreateBuilder(args);

// Add authentication and OBO support
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddMicrosoftIdentityWebApi(options =>
    {
        builder.Configuration.Bind("AzureAd", options);

        options.TokenValidationParameters.RoleClaimType = "roles";
    },
    options =>
    {
        builder.Configuration.Bind("AzureAd", options);
    });

// Enable downstream API calls (OBO)
builder.Services.AddMicrosoftGraph(
    builder.Configuration.GetSection("DownstreamApi"));

builder.Services.AddAuthorization();
builder.Services.AddControllers();

var app = builder.Build();

app.UseAuthentication();
app.UseAuthorization();

app.MapControllers();

app.Run();
```

### `appsettings.json`
```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "yourtenant.onmicrosoft.com",
    "TenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "ClientId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "ClientSecret": "YOUR_API_CLIENT_SECRET"
  },
  "DownstreamApi": {
    "BaseUrl": "https://graph.microsoft.com/v1.0/",
    "Scopes": "User.Read"
  }
}
```

### Example Controller — `Controllers/UserController.cs`
```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Graph;

namespace OboApi.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    [Authorize]
    public class UserController : ControllerBase
    {
        private readonly GraphServiceClient _graphServiceClient;

        public UserController(GraphServiceClient graphServiceClient)
        {
            _graphServiceClient = graphServiceClient;
        }

        [HttpGet("me")]
        public async Task<IActionResult> GetMe()
        {
            // Call Microsoft Graph on behalf of the user
            var me = await _graphServiceClient.Me.GetAsync();
            return Ok(me);
        }
    }
}
```

### How it works
1. **Client App (SPA / Mobile)** requests a token for your API (`api://{ClientId}/.default`).  
2. That token is sent in `Authorization: Bearer <token>` to your API.  
3. `Microsoft.Identity.Web` automatically redeems the incoming token using the **OBO flow** to obtain another token for the downstream API (e.g., Microsoft Graph).  
4. You can inject and use the `GraphServiceClient` or call any HTTP downstream API with `ITokenAcquisition`.

### Alternative: Call any downstream API (not Graph)
```csharp
using Microsoft.Identity.Web;

[ApiController]
[Route("api/[controller]")]
[Authorize]
public class DataController : ControllerBase
{
    private readonly ITokenAcquisition _tokenAcquisition;
    private readonly HttpClient _httpClient;

    public DataController(ITokenAcquisition tokenAcquisition, IHttpClientFactory httpClientFactory)
    {
        _tokenAcquisition = tokenAcquisition;
        _httpClient = httpClientFactory.CreateClient();
    }

    [HttpGet("downstream")]
    public async Task<IActionResult> CallDownstreamApi()
    {
        string[] scopes = new[] { "api://<downstream-api-client-id>/.default" };

        // Acquire OBO token
        var accessToken = await _tokenAcquisition.GetAccessTokenForUserAsync(scopes);

        _httpClient.DefaultRequestHeaders.Authorization =
            new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", accessToken);

        var response = await _httpClient.GetAsync("https://my-downstream-api/api/data");

        return Ok(await response.Content.ReadAsStringAsync());
    }
}
```

---

## 2) User
**Message:**  
elaborate point 1 of How it works about Client App(SPA) request a token for your API(api://{ClientID}.default). whose ClientID? Does SPA app needs to register itself in Entra ID?

---

## 2) Assistant
Great follow-up 👍 — the **“whose ClientId?”** question is at the heart of making the OBO flow work. Let me break it down carefully.

### OBO Flow Registration Setup
You need **two app registrations** in Entra ID (formerly Azure AD):

#### 1) Frontend (SPA / Mobile / Desktop) app registration
- **Yes**: your SPA (React/Angular/Blazor WASM, etc.) must be registered in Entra ID.  
- This registration represents the *client app* that the user signs into.  
- Configure **redirect URIs** and platform type (e.g., SPA with `https://localhost:3000`).  
- It gets its **own ClientId (ApplicationId)**, e.g. `SPA_ClientId = aaaa-bbbb-cccc-dddd`.
- The SPA requests a token **for the Web API**, not for itself.

#### 2) Backend Web API app registration
- Register your Web API in Entra ID.  
- Expose an **Application ID URI**, e.g. `api://bbbb-cccc-dddd-eeee`.  
- Define **scopes** (e.g., `user.read`, `access_as_user`).  
- It has its own **ClientId**, e.g. `API_ClientId = bbbb-cccc-dddd-eeee`.  
- The API registration must also have a **client secret or certificate** to call downstream APIs during OBO.

### How the Token Request Works (Point 1 clarified)
When the SPA app requests a token, it requests an access token for the **Web API app registration** (`API_ClientId`), not for itself.

**MSAL.js example from SPA:**
```javascript
const msalInstance = new msal.PublicClientApplication(msalConfig);

const loginRequest = {
  scopes: ["api://bbbb-cccc-dddd-eeee/access_as_user"] // The Web API scope
};

msalInstance.loginPopup(loginRequest).then((response) => {
  msalInstance.acquireTokenSilent(loginRequest).then((tokenResponse) => {
    // tokenResponse.accessToken is for the API, not the SPA
    callApi(tokenResponse.accessToken);
  });
});
```

- Here, `bbbb-cccc-dddd-eeee` = **Web API’s ClientId**, not SPA’s.  
- The SPA app uses its own `SPA_ClientId` when initializing MSAL, but acquires tokens for **Web API scopes**.

**Summary**
- **Yes**, the SPA needs its **own app registration** in Entra ID.  
- The SPA uses its own **ClientId** to initialize MSAL.  
- But when acquiring tokens, it requests them **for the API’s ClientId** (`api://<API_ClientId>/<scope>`).

---

## 3) User
**Message:**  
provide example content for the msalConfig used by your SPA code

---

## 3) Assistant
Perfect — let’s wire this up with a concrete **`msalConfig`** example 👇  

### Example `msalConfig` for your SPA
```javascript
const msalConfig = {
  auth: {
    clientId: "aaaa-bbbb-cccc-dddd", // <-- SPA app registration (Frontend ClientId)
    authority: "https://login.microsoftonline.com/<YOUR_TENANT_ID>", 
    redirectUri: "http://localhost:3000", // must match redirect URI registered in Entra ID
    postLogoutRedirectUri: "http://localhost:3000"
  },
  cache: {
    cacheLocation: "sessionStorage", // or "localStorage"
    storeAuthStateInCookie: false
  }
};

const loginRequest = {
  scopes: [
    "api://bbbb-cccc-dddd-eeee/access_as_user" 
    // This is the scope exposed by your Web API registration
  ]
};
```

### Key Notes
- `clientId` in `msalConfig` is the **SPA app registration’s Application (client) ID**.  
- `authority` is typically `https://login.microsoftonline.com/<TenantId>` (or `common` for multi-tenant).  
- `redirectUri` must exactly match the SPA’s configured redirect URI.  
- `scopes` point to your **Web API registration** (e.g., `api://<WebAPI_ClientId>/access_as_user`).

### Flow Recap
1. SPA signs in user via MSAL using its own **SPA ClientId**.  
2. SPA requests an **access token** for the Web API scope.  
3. Token is sent to your API.  
4. Your API then performs **OBO** to call downstream APIs.

---

## 4) User
**Message:**  
in the example you provided, the web API is called by a SPA client. suppose later on we have a daemon app that needs to call the web API app. since we already have the Entra ID app registration used by the SPA, e.g. ClientID = "aaaa-bbbb-cccc-dddd", can the daemon app use the same registered app to access web API? or the daemon app absolutely needs to have its own app registration?

---

## 4) Assistant
Excellent question 👏 — this gets into the heart of **“Can multiple app types share one app registration?”**  

**Short answer:** It’s technically possible to reuse the SPA registration, but **not recommended**. Prefer a **separate daemon app registration**.

### 1) SPA App (interactive, user-delegated tokens)
- Uses **public client** flow (no secret).  
- User signs in; tokens represent the user.  
- Authorization Code Flow with **PKCE**.

### 2) Daemon App (non-interactive, client credentials)
- Runs without a user.  
- Uses **client credentials** flow (client secret or certificate).  
- Calls your Web API **as itself** (app identity).

### Can they share the same registration?
**Option A: Reuse the SPA app registration** (possible but not ideal)  
- Add a client secret to the SPA registration; daemon uses it.  
- **Cons**: mixes public & confidential models, security risks, harder auditing.

**Option B: Separate registrations (recommended)** ✅  
- Register the daemon separately with its own **ClientId + secret/cert**.  
- Grant it **application permissions** (app role) to your Web API.  
- Clear separation for security, permissions, and audit logs.

**Best practice**
- SPA app → delegated permissions only.  
- Web API → exposes **scopes** (delegated) and **app roles** (application).  
- Daemon app → application permissions only.

---

## 5) User
**Message:**  
elaborate on this statement you provided "You can manage permissions separately (user-delegated vs. app-only).". provide more details on user-delegated vs app-only

---

## 5) Assistant
Great catch 👌 — the distinction between **user-delegated** and **app-only** permissions is crucial.

### User-delegated permissions
- **Who calls?** A user signs in; the app acts **on behalf of** the user.  
- **Flows:** Authorization Code (PKCE), OBO, Device Code.  
- **Token content:** includes user identity and `scp` (scopes), e.g.:
```json
{
  "scp": "access_as_user user.read",
  "upn": "jane@contoso.com"
}
```
- **Use case:** SPA calls Web API as Jane; API calls Graph `/me/messages` as Jane.  
- **Bound by:** granted scopes **and** the user’s own privileges.

### App-only permissions (Application permissions)
- **Who calls?** An application runs without a user (daemon/service).  
- **Flow:** Client Credentials.  
- **Token content:** no user claims; includes `roles` (app roles), e.g.:
```json
{
  "roles": ["App.ReadWrite.All"],
  "appid": "zzzz-yyyy-xxxx-wwww"
}
```
- **Use case:** Daemon syncs `/users` in Graph or hits your Web API as an app.  
- **Bound by:** the admin-granted **application permissions**.

### Why keep them separate?
- Clear auditing (user vs. app actions).  
- Principle of least privilege.  
- Avoid accidental app-only power in interactive clients.

**Analogy**  
- *Delegated* = “I’m authorizing the app to act as **me**.”  
- *App-only* = “This app is a **service account** with explicit privileges.”

---

**here’s how to configure your **Web API app registration** to support **both delegated (SPA)** and **app-only (daemon)** callers.

### Expose Delegated Permissions (Scopes)
1. **Expose an API** → **Add a scope**.  
2. Example:
   - Scope name: `access_as_user`
   - Who can consent: `Admins and users`
   - Admin consent display name: *Access the API on behalf of the signed-in user*
   - Value in token: `access_as_user`

**Resulting delegated token claims (example):**
```json
{
  "aud": "api://<WebAPI_ClientId>",
  "scp": "access_as_user",
  "upn": "jane@contoso.com"
}
```

### Expose Application Permissions (App Roles)
1. **App roles** → **Create app role**.  
2. Example role:
   - Display name: `Daemon API Access`
   - Value: `Daemon.Access`
   - Allowed member types: **Applications**
   - Description: *Allows a daemon service to access the API without a signed-in user*

**Resulting app-only token claims (example):**
```json
{
  "aud": "api://<WebAPI_ClientId>",
  "roles": ["Daemon.Access"],
  "appid": "xxxx-yyyy-zzzz"
}
```

### ASP.NET Authorization Example — Distinguish Delegated vs App-only
```csharp
[Authorize]
[HttpGet("whoami")]
public IActionResult WhoAmI()
{
    var user = HttpContext.User;

    // Delegated (SPA user) — check for scopes in 'scp' claim
    if (user.HasClaim(c => c.Type == "scp" && c.Value.Contains("access_as_user")))
    {
        var username = user.FindFirst("upn")?.Value ?? user.FindFirst("preferred_username")?.Value;
        return Ok($"Hello user {username}, you are calling with delegated permissions.");
    }
    // App-only (daemon) — check for app role in 'roles' claim
    else if (user.HasClaim(c => c.Type == "roles" && c.Value == "Daemon.Access"))
    {
        return Ok("Hello daemon app, you are calling with app-only permissions.");
    }

    return Forbid();
}
```

---


