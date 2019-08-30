:is-up-to-date: True

.. index:: Studio's Configuration Overrides, Configuration Overrides, Overrides

.. _studio-config-override:

================================
Studio's Configuration Overrides
================================

Crafter Studio comes with pre-configured settings that you may want to override.  To view the pre-configured settings in Crafter Studio, in your Authoring installation, go to ``webapps/studio/WEB-INF/classes/crafter/studio`` and open the file ``studio-config.yaml``.

To override any of the pre-configured settings, in your Authoring installation, go to ``shared/classes/crafter/studio/extension`` and add the settings you would like to configure in the file ``studio-config-override.yaml``.   The override file have some settings already listed that you may want to override in Crafter Studio:

--------------------------------
Content Repository Configuration
--------------------------------

The following section of Studio's configuration overrides allows you to do the following:

* ``studio.repo.basePath`` allows you to set your repository base
* ``studio.repo.siteSandboxBranch`` allows you to set the default branch to be used by sandbox
* ``studio.repo.published.live`` and ``studio.repo.published.staging`` allows you to set the branch for your environments

.. code-block:: yaml
   :caption: shared/classes/crafter/studio/extension/studio-config-override.yaml
   :linenos:

   ##################################################
   ##              Content Repository              ##
   ##################################################
   # Absolute or relative path to repository base (all actual repositories will be under this)
   studio.repo.basePath: ../data/repos
   # Sandbox Git repository branch for every site
   # studio.repo.siteSandboxBranch: master
   # If not using environment-config.xml, environments are configured here
   # Git repository branch for the `live` environment, default "live"
   # studio.repo.published.live: live
   # Git repository branch for the `staging` environment, default "staging"
   # studio.repo.published.staging: staging

------------------
Site Configuration
------------------

The following section of Studio's configuration overrides allows you to setup you site configuration

.. code-block:: yaml
   :caption: shared/classes/crafter/studio/extension/studio-config-override.yaml
   :linenos:

   ############################################################
   ##                   Site Configuration                   ##
   ############################################################
   # Destroy site context url for preview engine
   studio.configuration.site.preview.destroy.context.url: ${env:ENGINE_URL}/api/1/site/context/destroy.json?crafterSite={siteName}
   # Default preview URL
   studio.configuration.site.defaultPreviewUrl: ^https?://localhost:8080/?
   # Default authoring URL
   studio.configuration.site.defaultAuthoringUrl: ^https?://localhost:8080/studio/?

------------------------------
Preview Deployer Configuration
------------------------------

The following section of Studio's configuration overrides allows you to setup your deployer urls

.. code-block:: yaml
   :caption: shared/classes/crafter/studio/extension/studio-config-override.yaml
   :linenos:

   ############################################################
   ##                    Preview Deployer                    ##
   ############################################################

   # Default preview deployer URL (can be overridden per site)
   studio.preview.defaultPreviewDeployerUrl: ${env:DEPLOYER_URL}/api/1/target/deploy/{siteEnv}/{siteName}
   # Default preview create target URL (can be overridden per site)
   studio.preview.createTargetUrl: ${env:DEPLOYER_URL}/api/1/target/create_if_not_exists
   # Default preview create target URL (can be overridden per site)
   studio.preview.deleteTargetUrl: ${env:DEPLOYER_URL}/api/1/target/delete-if-exists/{siteEnv}/{siteName}
   # URL to the preview repository (aka Sandbox) where authors save work-in-progress
   studio.preview.repoUrl: ${env:CRAFTER_DATA_DIR}/repos/sites/{siteName}/sandbox



----------------------------
Preview Search Configuration
----------------------------

The following section of Studio's configuration overrides allows you to setup urls for search in preview

.. code-block:: yaml
   :caption: shared/classes/crafter/studio/extension/studio-config-override.yaml
   :linenos:

   ############################################################
   ##                   Preview Search                       ##
   ############################################################

   studio.preview.search.createUrl: ${env:SEARCH_URL}/api/2/admin/index/create
   studio.preview.search.deleteUrl: ${env:SEARCH_URL}/api/2/admin/index/delete/{siteName}

----------------------
Database Configuration
----------------------

The following section of Studio's configuration overrides allows you to setup the database url, port number, connection string to initialize the database and path

.. code-block:: guess
   :caption: shared/classes/crafter/studio/extension/studio-config-override.yaml
   :linenos:

   ##################################################
   ##                   Database                   ##
   ##################################################

   # Crafter Studio uses an embedded MariaDB by default
   # Crafter DB connection string
   studio.db.url: jdbc:mariadb://${env:MARIADB_HOST}:${env:MARIADB_PORT}/crafter?user=${env:MARIADB_USER}&password=${env:MARIADB_PASSWD}
   # Connection string used to initialize database. This creates the `crafter` schema, the `crafter` user and/or upgrades the database
   studio.db.initializer.url: jdbc:mariadb://${env:MARIADB_HOST}:${env:MARIADB_PORT}?user=${env:MARIADB_ROOT_USER}&password=${env:MARIADB_ROOT_PASSWD}
   # Connection string if using a database with an already created schema and user (like AWS RDS)
   # studio.db.initializer.url: ${studio.db.url}
   # Port number for the embedded database (note this must match what's in the connection URLs in this config file)
   studio.db.port: ${env:MARIADB_PORT}
   # Data folder for the embedded database
   studio.db.dataPath: ${env:MARIADB_DATA_DIR}
   # Socket path for the embedded database
   studio.db.socket: /tmp/MariaDB4j.${env:MARIADB_PORT}.sock

----------------------
Security Configuration
----------------------

The following section of Studio's configuration overrides allows you to randomize the admin password on a fresh install (for more information, see: :ref:`randomize-admin-password`), configure encryption and configure authentication method to be used (for more information, see: :ref:`configuring-studio-security`).

.. code-block:: yaml
   :caption: shared/classes/crafter/studio/extension/studio-config-override.yaml
   :linenos:

   ##################################################
   ##                   Security                   ##
   ##################################################
   # Enable random admin password generation
   # studio.db.initializer.randomAdminPassword.enabled: false
   # Random admin password length
   # studio.db.initializer.randomAdminPassword.length: 16
   # Random admin password allowed chars
   # studio.db.initializer.randomAdminPassword.chars: ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*_=+-/

   # HTTP Session timeout for studio (value is in minutes).
   # studio.security.sessionTimeout: 60

   # Salt for encrypting
   # studio.security.cipher.salt: TDEsDF8vx3gV4c7G

   # Key for encrypting
   # studio.security.cipher.key: AoCcBdnsTa9R3DdG

   # Enable password requirements validation
   # studio.security.passwordRequirements.enabled: false
   # Password requirements validation regular expression
   #   (?=.*[0-9])       a digit must occur at least once
   #   (?=.*[a-z])       a lower case letter must occur at least once
   #   (?=.*[A-Z])       an upper case letter must occur at least once
   #   (?=.*[@#$%^&+=])  a special character must occur at least once
   #   (?=\S+$)          no whitespace allowed in the entire string
   #   .{8,}             anything, at least eight places though
   # studio.security.passwordRequirements.validationRegex: ^(?=.*[0-9])(?=.*[a-z])(?=.*[A-Z])(?=.*[@#$%^&+=])(?=\S+$).{8,}$

   # Studio authentication chain configuration
   # studio.authentication.chain:
     # Authentication provider type
     # - provider: HEADERS
       # Authentication via headers enabled
       # enabled: false
       # Authentication header for secure key
       # secureKeyHeader: secure_key
       # Authentication headers secure key that is expected to match secure key value from headers
       # Typically this is placed in the header by the authentication agent, e.g. Apache mod_mellon
       # secureKeyHeaderValue: secure
       # Authentication header for username
       # usernameHeader: username
       # Authentication header for first name
       # firstNameHeader: firstname
       # Authentication header for last name
       # lastNameHeader: lastname
       # Authentication header for email
       # emailHeader: email
       # Authentication header for groups: comma separated list of sites and groups
       #   Example:
       #   site_author,site_xyz_developer
       # groupsHeader: groups
       # Enable/disable logout for headers authenticated users (SSO)
       # logoutEnabled: false
       # If logout is enabled for headers authenticated users (SSO), set the endpoint of the SP or IdP logout, which should
       # be called after local logout. The {baseUrl} macro is provided so that the browser is redirected back to Studio
       # after logout (https://STUDIO_SERVER:STUDIO_PORT/studio)
       # logoutUrl: /mellon/logout?ReturnTo={baseUrl}
     # Authentication provider type
     # - provider: LDAP
       # Authentication via LDAP enabled
       # enabled: false
       # LDAP Server url
       # ldapUrl: ldap://localhost:389
       # LDAP bind DN (user)
       # ldapUsername: cn=Manager,dc=my-domain,dc=com
       # LDAP bind password
       # ldapPassword: secret
       # LDAP base context (directory root)
       # ldapBaseContext: dc=my-domain,dc=com
       # LDAP username attribute
       # usernameLdapAttribute: uid
       # LDAP first name attribute
       # firstNameLdapAttribute: cn
       # LDAP last name attribute
       # lastNameLdapAttribute: sn
       # Authentication header for email
       # emailLdapAttribute: mail
       # LDAP groups attribute
       # groupNameLdapAttribute: crafterGroup
       # LDAP groups attribute name regex
       # groupNameLdapAttributeRegex: .*
       # LDAP groups attribute match index
       # groupNameLdapAttributeMatchIndex: 0
   # Authentication provider type
     # - provider: DB
       # Authentication via DB enabled
       # enabled: true



------------------
Mail Configuration
------------------

The following section of Studio's configuration overrides allows you to setup the SMTP server to be used by Crafter CMS when sending emails

.. code-block:: yaml
   :caption: shared/classes/crafter/studio/extension/studio-config-override.yaml
   :linenos:

   ##################################################
   ##        SMTP Configuration (Email)            ##
   ##################################################

   # Default value for from header when sending emails.
   # studio.mail.from.default: admin@example.com
   # SMTP server name to send emails.
   studio.mail.host: ${env:MAIL_HOST}
   # SMTP port number to send emails.
   studio.mail.port: ${env:MAIL_PORT}
   # SMTP username for authenticated access when sending emails.
   # studio.mail.username:
   # SMTP password for authenticated access when sending emails.
   # studio.mail.password:
   # Turn on/off (value true/false) SMTP authenaticated access protocol.
   # studio.mail.smtp.auth: false
   # Enable/disable (value true/false) SMTP TLS protocol when sending emails.
   # studio.mail.smtp.starttls.enable: false
   # Enable/disable (value true/false) SMTP EHLO protocol when sending emails.
   # studio.mail.smtp.ehlo: true
   # Enable/disable (value true/false) debug mode for email service. Enabling debug mode allows tracking/debugging communication between email service and SMTP server.
   # studio.mail.debug: false

----------
Clustering
----------

The following section of Studio's configuration overrides allows you to setup Studio for clustering

.. code-block:: yaml
   :caption: shared/classes/crafter/studio/extension/studio-config-override.yaml
   :linenos:

   #-----------------------------------------------------------------------------
   # IMPORTANT: When enabling clustering, please specify the environment variable
   # SPRING_PROFILES_ACTIVE=crafter.studio.externalDb in your crafter-setenv.sh
   # (or Docker/Kubernetes env variables). This will stop studio from starting
   # its embedded DB. Also configure the appropiate MARIADB env variables
   # -----------------------------------------------------------------------------

   # Cluster Syncers
   # Sandbox Sync Job interval in milliseconds which is how often to sync the work-area
   # studio.clustering.sandboxSyncJob.interval: 2000
   # Published Sync Job interval in milliseconds which is how often to sync the published repos
   # studio.clustering.publishedSyncJob.interval: 60000
   # Cluster member after heartbeat stale for amount of minutes will be declared inactive
   # studio.clustering.heartbeatStale.timeLimit: 5
   # Cluster member after being inactive for amount of minutes will be removed from cluster
   # studio.clustering.inactivity.timeLimit: 5

   # Cluster member registration, this registers *this* server into the pool
   # Cluster node registration data, remember to uncomment the next line
   # studio.clustering.node.registration:
   #  this server's local address (reachable to other cluster members)
   #  localAddress: ${env:CLUSTER_NODE_ADDRESS}
   #  authentication type to access this server's local repository
   #  possible values
   #   - none (no authentication needed)
   #   - basic (username/password authentication)
   #   - key (ssh authentication)
   #  authenticationType: none
   #  username to access this server's local repository
   #  username: user
   #  password to access this server's local repository
   #  password: SuperSecurePassword
   #  private key to access this server's local repository (multiline string)
   #  privateKey: |
   #    -----BEGIN PRIVATE KEY-----
   #    privateKey
   #    -----END PRIVATE KEY-----


----
CORS
----

The following section of Studio's configuration overrides allows you to setup CORS

.. code-block:: yaml
   :caption: shared/classes/crafter/studio/extension/studio-config-override.yaml
   :linenos:

   # This is configured as permissive by default for ease of deployment
   # Remember to tighten this up for production

   # Disable CORS headers completely
   # studio.cors.disable: false
   # Value for the Access-Control-Allow-Origin header
   # studio.cors.origins: '*'
   # Value for the Access-Control-Allow-Headers header
   # studio.cors.headers: '*'
   # Value for the Access-Control-Allow-Methods header
   # studio.cors.methods: '*'
   # Value for the Access-Control-Allow-Credentials header
   # studio.cors.credentials: true
   # Value for the Access-Control-Max-Age header
   # studio.cors.maxage: -1
   # The active environment for multi environment configuration, e.g. qa, prod, dev
   # studio.configuration.environment.active: ENV

------
Search
------

The following section of Studio's configuration overrides allows you to setup the url for search

.. code-block:: yaml
   :caption: shared/classes/crafter/studio/extension/studio-config-override.yaml
   :linenos:

   # URLs to connect to Elasticsearch
   studio.search.urls: ${env:ES_URL}
