An example MVC application authenticating against Azure AD using WSFederation without OWIN middleware.

This example is an old way of doing things and I'm providing this for others who might have to migrate old code to use Azure AD but don't have time to rework their code. OWIN middleware is now the preferred way of handling the authentication.

I've based this example on Thinktecture's IdentityServer 2 sample, MVC and WCF RP (SAML), with a slight change to use Azure AD and no ClaimsService. Thank you to Thinktecture for keep those samples available and for all the work they do. https://github.com/IdentityServer/IdentityServer2/tree/master/samples 


Azure AD Configuration

1. Create a new App Registration in your Azure AD Tenant.
2. Under the App's settings set the App ID URI to whatever you want. I set it to http://samlpoc
3. Set a reply URL to that of your local website, in my case "https://localhost:44398/". I used IIS Express.

4. Set the audience URI's in identity.config to that of your reply URL and APP ID

```

<audienceUris>
    <add value="https://localhost:44398/" />
    <add value="http://samlpoc"/>
</audienceUris>

```

5. Update the identityServices.config with the issuer which you will find by clicking the Endpoints button in the Azure AD App Registration blade. The realm is the APP ID URI and the reply URL should match (note the trailing /)

```

<wsFederation passiveRedirectEnabled="true"
                  issuer="https://login.windows.net/dcd5d5df-cb89-42a7-85cc-e3b113d4aad1/wsfed"
                  realm="http://samlpoc"
                  reply="https://localhost:44398/"
                  requireHttps="true" />
                  
```
