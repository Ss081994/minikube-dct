# Test SAML IDP Setup for DCT

This guide walks you through setting up a test SAML Identity Provider (IDP) and configuring SAML-based Single Sign-On (SSO) for a Delphix Continuous Testing (DCT) instance.

---

## Step 1: Download and Run the Test IDP

```bash
 dct_host=local.dct
 docker pull kristophjunge/test-saml-idp
 docker run -p 8082:8080 -p 8443:8443 \
  -e SIMPLESAMLPHP_SP_ENTITY_ID=saml-poc \
  -e SIMPLESAMLPHP_SP_ASSERTION_CONSUMER_SERVICE=https://$dct_host/v2/saml/SSO \
  kristophjunge/test-saml-idp:latest
```
## Step 2: Configure SAML at DCT
 
 1. Get the metadata from the IDP
   ```sh
   curl http://localhost:8082/simplesaml/saml2/idp/metadata.php | sed 's/HTTP-Redirect/HTTP-POST/g'  
   ```
 2. Get the XML and paste in the Configure DCT SSO :
    ```sh
    ●	Enable -> true
    ●	Auto create users -> true
    ●	Entity ID -> saml-poc (must match the value chosen in the docker run command above)
    ●	Metadata (paste value from clipboard)
    ```
3. Login and Test
    Open an incognito and test 
   ●	Username: user1
   ●	Password: user1pass
   
