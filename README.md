# Reproducible Slim debugging environment
This is supposed to be a reproducible devcontainer environment to debug Slim / dicom-microscopy-viewer from Visual Studio Code. 

## Requirements
The only requirement necessary is Visual Studio Code with the following extensions: 
- Dev Containers
- Container Tools
- Docker

## Set-up the debugging environment
Please execute the following steps: 
1. Clone this repository using `git clone --recurse-submodules <REPO_URL>`
2. Open this repository in Visual Studio Code. 
3. Press `strg+shift+P`, which will open a dropdown menu from the top. Type "Dev Containers: Rebuild and open" to find and then select "Dev Containers: Rebuild and Reopen in Container". This will build the Dev Container. 
4. Modify slim/public/config/local.js by adding the following snippet at the top, replacing respective path and servers: 

```
  // This must match the location configured for web server
  path: '/',
  servers: [
    {
      id: 'bmdeep',
      // This must match the proxy location configured for the web server
      url: 'https://healthcare.googleapis.com/v1beta1/projects/idc-external-031/locations/northamerica-northeast1/datasets/bmdeep/dicomStores/twocasesample/dicomWeb',
      write: true
    }
  ],
  oidc: {
    authority: "https://accounts.google.com",
    clientId: "293449031882-k4um45hl4g94fsgbnviel0lh38836i9v.apps.googleusercontent.com",
    scope:
      "email profile openid https://www.googleapis.com/auth/cloud-healthcare",
    grantType: "implicit",
    endSessionEndpoint: "https://www.google.com/accounts/Logout",
  },
```

5. Now open up a terminal and execute: 
    a. `cd dicom-microcopy-viewer && yarn install` 
    b. `yarn link`
    c. `cd ../slim && yarn link dicom-microscopy-viewer`
    d. `cd ../dicom-microscopy-viewer && yarn webpack:dynamic-import:watch`

6. Open a second terminal and execute: 
    a. `cd slim && yarn start`