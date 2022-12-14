# Custom Keycloak Theme Development (Starter Pack)
Starter setup for development of custom keycloak themes (e.g. login theme) or customization of existing themes. This project is based on Keycloak 20.x.x.

## Requirements:
- Docker

## Description:
- Provided docker-compose.yml file can be used to build development environment for custom keycloak themes development. Configuration will create postgres database container with persistent data volume (to not lose keycloak configuration in case container is recreated). Additionally it will create keycloak identity server container which is configured for development purposes (basic auth, no authentication for postgresql) and runs in development mode (default mode). 
- Folder ./themes/ is mounted in Keycloak identity server to immediately apply all changes made in ./themes/ folder to keycloak server. 
- Remember when deploying themes to production keycloak server (server which runs in production mode) you will have to restart keycloak server for changes to take effect. 
- Example of custom theme named "custom_theme_example" is provided and will be automatically loaded into keycloak as described above.

## Build Development environment
- To build keycloak identity server from docker-compose.yml file position yourself in project directory and run:
```
docker compose up -d
```
- After a short time keycloak administration console should be available using the next link: http://localhost:8888
 - Default credentials: admin/admin

### Configure Keycloak server to use provided example
This procedure should be performed first time you build keycloak identity server from provided docker-compose.yml file. Changes will be persisted to .postgres_data folder and automatically applied next time you rebuild it.
1. Create new realm
2. Make sure that newly created realm is selected. All subsequent changes should be done in newly created realm.
3. Create a new client with default settings. For easier testing set valid redirect URLs and web origins to *
<img width="300px" src="https://raw.githubusercontent.com/Andrej988/keycloak-custom-theme-dev/main/img-keycloak-client-setup.JPG">

4. Create test user. Don't forget to set a password.
5. Open realm settings and choose Themes tab. Change login theme to custom_theme_example.
<img width="300px" src="https://raw.githubusercontent.com/Andrej988/keycloak-custom-theme-dev/main/img-keycloak-realm-theme-setting.JPG">

6. Now login screen for newly created realm will use custom theme.

<img width="300px" src="https://raw.githubusercontent.com/Andrej988/keycloak-custom-theme-dev/main/img-login-screen.JPG">

## Notes for development
- You have to follow a strict structure in ./themes/ folder. 
  - First subfolder level represents new theme and it is used as a theme name. e.g. ./themes/custom_theme/ theme name in keycloak would be "custom_theme"
  - Second subfolder level represents theme type. One theme can have multiple types. e.g. ./themes/custom_theme/login/
    - Possible types:
      - Account management: account
      - Admin console: admin
      - Emails: email
      - Login forms: login
      - Welcome page: welcome
- You can find existing keycloak resources (templates) which can be modified in default keycloak themes https://github.com/keycloak/keycloak/tree/main/themes/src/main/resources/theme

## Additional information
- Keycloak Server Development / Themes Documentation
  - https://www.keycloak.org/docs/latest/server_development/#_themes
  - https://github.com/keycloak/keycloak-documentation/blob/main/server_development/topics/themes.adoc
- You can also use keycloakify for theme customization: https://docs.keycloakify.dev/


