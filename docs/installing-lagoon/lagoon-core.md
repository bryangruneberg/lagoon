# Install Lagoon Core

1. Add Lagoon Charts repository to your Helm:&#x20;
   1. `helm repo add lagoon https://uselagoon.github.io/lagoon-charts/`
2. Create a directory for the configuration files we will create, and make sure that it’s version controlled. Ensure that you reference this path in commands referencing your `values.yml` files.&#x20;
   1. Create `values.yml` in the directory you’ve just created. Example: [https://gist.github.com/Schnitzel/58e390bf1b6f93117a37a3eb02e8bae3](https://gist.github.com/Schnitzel/58e390bf1b6f93117a37a3eb02e8bae3)&#x20;
   2. Update the endpoint URLs (change them from api.lagoon.example.com to your values).
3. Now run `helm upgrade --install` command, pointing to `values.yml`, like so:\
   ****`helm upgrade --install --create-namespace --namespace lagoon-core -f values.yml lagoon-core lagoon/lagoon-core`
4. Lagoon Core is now installed! :tada:&#x20;
5. Visit the Keycloak dashboard at the URL you defined in the `values.yml` for Keycloak.
   1. Click Administration Console
   2. Username: `admin`
   3. Password: use `lagoon-core-keycloak` secret, key-value `KEYCLOAK_ADMIN_PASSWORD`
   4. Retrieve the secret like so:`kubectl -n lagoon-core get secret lagoon-core-keycloak -o jsonpath="{.data.KEYCLOAK_ADMIN_PASSWORD}" | base64 --decode`
   5. Click on **User** on top right.
      1. Go to **Manage Account.**
      2. Add an **Email** for the admin account you created.
      3. Save.
   6. Go to **Realm Lagoon** -> **Realm Settings** -> **Email**
      1. Configure email server for Keycloak, test connection via “Test connection” button.
   7. Go to **Realm Lagoon** -> **Realm Settings** -> **Login**
      1. Enable “Forgot Password”
      2. Save.
   8. You should now be able to visit the Lagoon UI at the URL you defined in the `values.yml` for the UI.
      1. Username: `lagoonadmin`
      2. Secret: use `lagoon-core-keycloak` secret key-value: `LAGOON-CORE-KEYCLOAK`
      3. Retrieve the secret:` kubectl -n lagoon-core get secret lagoon-core-keycloak -o jsonpath="{.data.KEYCLOAK_LAGOON_ADMIN_PASSWORD}" | base64 --decode`

!!! warning "Warning:"
    Note: Sometimes we run into Docker Hub pull limits. We are considering moving our images elsewhere if this continues to be a problem.&#x20;

!!! Note "Note:"
    Note: Currently Lagoon only supports one Lagoon per cluster - meaning you can’t currently split your dev/test/prod environments across separate clusters, but this is something we are looking to implement in the future.&#x20;