# Instructions:

## Cross reference table:
    use js script packages you think necessary, create two sections in the OAuth-Index.htm file: 
    1. a section for a cross reference table 
    2. a section for a content display panel. position this content display panel section below the cross reference table section

    Include the "# Field" blocks in this file and MSAL-Sample.md and MSAL-Package.md. Match up the tag 

## Cross reference table structure:

    The table consists of columns made of these Types:

        Type: [Diagram, Request, Response, Configuration, Sample, Package]

        Each "# Field" block contains a Type tag which should be used to determine which column the block's content should be placed.

    The table stars with the first column called Flow which contains the rows each of which of a Flow type:

        Flow: [OIDC, Auth code, Client credential, Device code, Implicit, On-behalf-of, Uid-Pwd-ROPC, Integ Win Auth-IWA, Auth Bearer]

        Each "# Fideld" block contains a Flow tag which should be used to determine which row the block belongs to.

        The field of a row should contain the collection of the content from "Application" tag of the columns 

    In summary, the cross reference table is made of columns of Type field in "# Field" and the rows of Flow field in "# Field"

    The Application tag in "# Field" block is descriptive of the possible application types a Flow can appear.

        Application: [Desktop, Mibile, Single-page-SPA, Web, Web-API, Daemon]
        
    Some "# Field" blocks with Type: Package do not have Flow tag. Match the blocks using the Application tag of the block with Type: Package with the Application tag of the block with Type: Sample. For example, these "# Field" blocks match:

        === begin of match example
            # Field
                - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-protocols-oidc#protocol-flow-sign-in
                - display: OIDC diagram
                - description: OIDC diagram for ID token
                - Flow: OIDC
                - Application: Web
                - Type: Diagram

            # Field
                - URL: https://learn.microsoft.com/en-us/entra/identity-platform/reference-v2-libraries#web-application
                - display: Web 
                - description: Web Application
                - Application: Web
                - Type: Package
        === end of match example

## Cross reference table field content:
    inside the "# Field" block with the column and the row of Flows. Each field of the cross reference table should contain a web link that consists of the text from the content of "display" tag, and the URL from the content of "URL" tag

## Supported user behavior:
    when the user clicks the web link in a field, the web page should be displayed in the content display panel

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-protocols-oidc#protocol-flow-sign-in
    - display: OIDC diagram
    - description: OIDC diagram for ID token
    - Flow: OIDC
    - Application: Web
    - Type: Diagram

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-protocols-oidc#enable-id-tokens
    - display: OIDC ID token configuration
    - description: OIDC ID token configuraton sample, enable ID token
    - Flow: OIDC
    - Application: Web
    - Type: Configuration

### Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-protocols-oidc#send-the-sign-in-request
    - display: OIDC ID token request
    - description: OIDC ID token request sample
    - Flow: OIDC
    - Application: Web
    - Type: Request

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-protocols-oidc#successful-response
    - display: OIDC ID token response
    - description: OIDC ID token response sample
    - Flow: OIDC
    - Application: Web
    - Type: Response

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-auth-code-flow#protocol-details
    - display: Auth code flow
    - description: Auth code flow diagram
    - Flow: Auth code
    - Application: SPA, Web, Desktop, Mobile
    - Type: Diagram

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-auth-code-flow#request-an-authorization-code
    - display: Auth code request
    - description: Auth code flow request
    - Flow: Auth code
    - Application: SPA, Web, Desktop, Mobile
    - Type: Request

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-auth-code-flow#successful-response
    - display: Auth code response
    - description: Auth code flow response
    - Flow: Auth code
    - Application: SPA, Web, Desktop, Mobile
    - Type: Response

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-auth-code-flow#request-an-id-token-as-well-or-hybrid-flow
    - display: Auth code and ID token request
    - description: Auth code and ID token flow request
    - Flow: Auth code
    - Application: SPA, Web, Desktop, Mobile
    - Type: Request

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-auth-code-flow#successful-response-1
    - display: Auth code and ID token response
    - description: Auth code and ID token flow response
    - Flow: Auth code
    - Application: SPA, Web, Desktop, Mobile
    - Type: Response

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-auth-code-flow#request-an-access-token-with-a-client_secret
    - display: access token request
    - description: access token request
    - Flow: Auth code
    - Application: SPA, Web, Desktop, Mobile
    - Type: Request    

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-auth-code-flow#successful-response-2
    - display: access token response
    - description: access token response
    - Flow: Auth code
    - Application: SPA, Web, Desktop, Mobile
    - Type: Response   

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-auth-code-flow#refresh-the-access-token
    - display: refresh access token request
    - description: refresh access token request
    - Flow: Auth code
    - Application: SPA, Web, Desktop, Mobile
    - Type: Request    

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-auth-code-flow#successful-response-3
    - display: refresh access token response
    - description: refresh access token response
    - Flow: Auth code
    - Application: SPA, Web, Desktop, Mobile
    - Type: Response    


# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-client-creds-grant-flow#protocol-diagram
    - display: Client credential diagram
    - description: Client credential diagram
    - Flow: Client credential
    - Application: Daemon
    - Type: Diagram    

    
# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-client-creds-grant-flow#get-a-token
    - display: Client credential request 
    - description: Client credential request access token
    - Flow: Client credential
    - Application: Daemon
    - Type: Request  


# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-client-creds-grant-flow#successful-response-1
    - display: Client credential response 
    - description: Client credential response for access token
    - Flow: Client credential
    - Application: Daemon
    - Type: Response  


# Field
    - URL: https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2
    - display: Client credential .NET Core app sample 
    - description: Client credential .NET Core application sample 
    - Flow: Client credential
    - Application: Daemon
    - Type: Code  

# Field
    - URL: https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2
    - display: Client credential .NET MVC app sample 
    - description: Client credential .NET MVC application sample 
    - Flow: Client credential
    - Application: Daemon
    - Type: Code  

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-device-code#protocol-diagram
    - display: Device code flow 
    - description: Device code flow 
    - Flow: Device code
    - Application: Daemon
    - Type: Diagram  

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-device-code#device-authorization-request
    - display: Device code authorization request 
    - description: Device code flow authorization request 
    - Flow: Device code
    - Application: Daemon
    - Type: Request  


# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-device-code#device-authorization-response
    - display: Device code authorization response 
    - description: Device code flow authorization response 
    - Flow: Device code
    - Application: Daemon
    - Type: Response  

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-device-code#authenticating-the-user
    - display: Device code user authentication request 
    - description: Device code flow user authentication request 
    - Flow: Device code
    - Application: Daemon
    - Type: Request  

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-device-code#successful-authentication-response
    - display: Device code user authentication response 
    - description: Device code flow user authentication response 
    - Flow: Device code
    - Application: Daemon
    - Type: Response  


# Field
    - URL: https://github.com/Azure-Samples/ms-identity-dotnet-desktop-tutorial/tree/master/4-DeviceCodeFlow#register-the-client-app-console-devicecodeflow-multitarget-v2
    - display: Console app 
    - description: Device code flow for Console app registration
    - Flow: Device code
    - Application: Desktop
    - Type: Configuration  

# Field
    - URL: https://github.com/Azure-Samples/ms-identity-dotnet-desktop-tutorial/tree/master/4-DeviceCodeFlow
    - display: Desktop code 
    - description: Device code app registration
    - Flow: Device code
    - Application: Desktop
    - Type: Sample  

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-on-behalf-of-flow#protocol-diagram
    - display: On-Behalf-Of flow
    - description: On-Behalf-Of flow diagram 
    - Flow: On-Behalf-Of
    - Application: Web, Web-API, Node-JS
    - Type: Diagram 

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-on-behalf-of-flow#middle-tier-access-token-request
    - display: On-Behalf-Of request
    - description: On-Behalf-Of request 
    - Flow: On-Behalf-Of
    - Application: Web, Web-API, Node-JS
    - Type: Request 


# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-on-behalf-of-flow#middle-tier-access-token-response
    - display: On-Behalf-Of response
    - description: On-Behalf-Of response 
    - Flow: On-Behalf-Of
    - Application: Web, Web-API, Node-JS
    - Type: Response

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-on-behalf-of-flow#obtain-a-saml-token-by-using-an-obo-request-with-a-shared-secret
    - display: On-Behalf-Of SAML assertion request
    - description: On-Behalf-Of SAML assertion request
    - Flow: On-Behalf-Of
    - Application: Web, Web-API, Node-JS
    - Type: Request    

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-on-behalf-of-flow#response-with-saml-assertion
    - display: On-Behalf-Of SAML assertion response
    - description: On-Behalf-Of SAML assertion response
    - Flow: On-Behalf-Of
    - Application: Web, Web-API, Node-JS
    - Type: Response

# Field
    - URL: https://learn.microsoft.com/azure/active-directory/develop/quickstart-configure-app-expose-web-apis
    - display: Web API configuration
    - description: Web API configuration
    - Flow: On-Behalf-Of
    - Application: Web-API
    - Type: Configuration


# Field
    - URL: https://github.com/Azure-Samples/ms-identity-dotnetcore-ca-auth-context-app/blob/main/README.md#step-2-alternative-manual-setup-choose-the-azure-ad-tenant-where-you-want-to-create-your-applications
    - display: Web
    - description: Web app configuration
    - Flow: Client credential
    - Application: Web
    - Type: Configuration

# Field
    - URL: https://github.com/Azure-Samples/ms-identity-dotnetcore-ca-auth-context-app/blob/main/README.md#step-2-alternative-manual-setup-choose-the-azure-ad-tenant-where-you-want-to-create-your-applications
    - display: Web 
    - description: Web app 
    - Flow: Client credential
    - Application: Web
    - Type: Sample


# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-implicit-grant-flow#protocol-diagram 
    - display: Implicit flow diagram
    - description: Implicit flow diagram
    - Flow: Implicit
    - Application: SPA, Web
    - Type: Diagram

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-implicit-grant-flow#send-the-sign-in-request
    - display: Implicit flow request
    - description: Implicit flow request
    - Flow: Implicit
    - Application: SPA, Web
    - Type: Request

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-implicit-grant-flow#successful-response
    - display: Implicit flow response
    - description: Implicit flow response
    - Flow: Implicit
    - Application: SPA, Web
    - Type: Response

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth-ropc#protocol-diagram
    - display: ROPC flow diagram
    - description: ROPC flow diagram
    - Flow: Uid-Pwd-ROPC
    - Application: Console, Daemon
    - Type: Diagram

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth-ropc#authorization-request
    - display: ROPC flow request
    - description: ROPC flow request
    - Flow: Uid-Pwd-ROPC
    - Application: Console, Daemon
    - Type: Request

# Field
    - URL: https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth-ropc#successful-authentication-response
    - display: ROPC flow response
    - description: ROPC flow response
    - Flow: Uid-Pwd-ROPC
    - Application: Console, Daemon
    - Type: Response

# Field
    - URL: https://github.com/azure-samples/active-directory-dotnetcore-devicecodeflow-v2#register-the-client-app-active-directory-dotnet-deviceprofile
    - display: Browerless (Headless)
    - description: Browerless (Headless) Application
    - Flow: Device code
    - Application: Browerless
    - Type: Configuration

# Field
    - URL: https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/master/4-WebApp-your-API/4-1-MyOrg/README.md#register-the-client-app-todolistclient-aspnetcore-webapi
    - display: Web
    - description: Web Application
    - Flow: Auth code
    - Application: Web
    - Type: Configuration    
    
# Field
    - URL: https://github.com/Azure-Samples/ms-identity-dotnet-desktop-tutorial/tree/master/1-Calling-MSGraph/1-1-AzureAD#register-the-client-app-console-interactive-multitarget-v2
    - display: Desktop app
    - description: Desktop Application
    - Flow: Auth code
    - Application: Web
    - Type: Configuration 
