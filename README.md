## Cloudflare Web Pages

This Module helps us deploy a static page using Cloudflare Pages. The module requires proper credentials before use.

Supported Frameworks:
* Astro
* Angular
* Hugo
* Next.js
* React
* Vue


### Getting Started

First, configure your ["account id"](#steps-to-get-your-account-id)  and ["api token"](#steps-to-create-a-cloudflare-api-token) properly in Cloudflare and add the secrets to GitHub Actions.

Then create a GitHub Action to create the static web deployment and add the Snow Monkeys reusable workflows as shown:

```yaml
name: Deploy static webpage in Cloudflare Pages

on:
  push:
    branches: [ "main" ]

jobs:
  deploy-static-webpage:
    uses: snow-monkeys-modules/cloudflare-static-website/.github/workflows/main.yml@main
    with:
      framework: react
      project-name: hello-react-vite
      build-directory: dist
      build-command: npm run build
    secrets:
      account-id: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
      cloudflare-token: ${{ secrets.CLOUDFLARE_API_TOKEN_PAGES }}
      snow-token: ${{ secrets.SNOW_TOKEN }}

  use-output:
    needs: deploy-static-webpage
    runs-on: ubuntu-22.04
    steps:
     - name: Use output
       run: echo "Url is ${{ needs.deploy-static-webpage.outputs.static_webpage_url }}"


```


Required inputs:

- [`framework`](#required-inputs) (String) The framework is based the application. Available value: "react", "angular", "astro", "next.js".
- [`project-name`](#required-inputs) (String) Project name also used as application name for Cloudflare Pages.

Optional inputs:

- [`build-directory`](#optional-inputs) (String) The build directory in framework. Default: "dist" 
- [`build-command`](#optional-inputs) (String) Build command to generate the distribution javascript bundle. Default: "npm run build"

Secrets:

- [`account-id`](#secrets) (String) Cloudflare account id.
- [`cloudflare-token`](#secrets) (String) Cloudflare API token with proper permissions.
- [`snow-token`](#secrets) (String) Cloudflare API token with proper permissions.

Outputs:

- [`static_webpage_url`](#outpust) (String) url.


You can review your deployed statics web pages in Cloudflare dashboard workers-and-pages

#### Steps to get your account ID

1. **Log in** to your [Cloudflare Dashboard](https://dash.cloudflare.com/).
    
2. On the **homepage**, you’ll see the account id in the dropdown menu next to your account.

![[accountid.png]]
    
3. Copy and paste into Github actions secrets.


#### Steps to create a Cloudflare API Token

1. **Log in** to your[Cloudflare Dashboard](https://dash.cloudflare.com/).
    
2. **Navigate to API Tokens**
    
    - Click on your profile icon (top-right corner).
        
    - Select **"Profile"**.
        
    - Go to the **"API Tokens"** tab.
        
3. **Create a New API Token** 
    
    - Click **"Create Token"**.
        
    - You can either:
        
        - Use a **predefined template** (for Cloudflare pages is required permission for Cloudflare Workers)
            
        - Create a **custom token**.
            
4. **Configure Token Permissions**
    
    - **For a Custom Token:**
        
        - Select the **specific resources**.
            
        - Define **permissions** (e.g., Read, Edit, Delete).
            
        - Restrict token access to specific zones if needed.
            
5. Set Token TTL **(Optional)**
    
    - You can set an expiration time (TTL) for the token if needed.
        
6. Define IP Address Filtering **(Optional)**
    
    - Restrict the token to specific IP addresses for security.
        
7. **Generate the Token**
    
    - Click **"Continue to summary"**.
        
    - Review permissions.
        
    - Click **"Create Token"**.
        
8. **Copy and Store the Token Securely**
    
    - The token will appear only once—**copy and save it securely**.
        
    - If lost, you’ll need to generate a new one.
    
9. **Copy and paste** into Github actions secrets.
