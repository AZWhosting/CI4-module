[🇺🇸 English](#en) | [🇫🇷 Français](#fr)
## FR
# Tutoriel : Créer un système modulaire dans CodeIgniter 4

Ce tutoriel vous guide à travers la création d'un système modulaire dans une application CodeIgniter 4, y compris la mise en place de routes, de l'autoload et des contrôleurs spécifiques aux modules.

## Étapes

1. **Créer le dossier "modules" à la racine de votre projet CodeIgniter**

    Organisez vos dossiers pour inclure un nouveau dossier modules à la racine du projet. La structure devrait ressembler à cela :

    ```
    /CodeIgniter
    |-- /app
    |-- /modules  # Dossier pour les modules
    |-- /public
    |-- /system
    |-- /writable
    ```

2. **Modifier les routes dans app/Config/Routes.php**

    Ajoutez le code suivant pour inclure dynamiquement les fichiers de configuration des routes de chaque module.

    ```php
    /**
     * ---------------------------------------------------------------
     * Include Modules Routes Files
     * ---------------------------------------------------------------
     */
    if (file_exists(ROOTPATH . 'modules')) {
        $modulesPath = ROOTPATH . 'modules/';
        $modules = scandir($modulesPath);

        foreach ($modules as $module) {
            if ($module === '.' || $module === '..') continue;
            if (is_dir($modulesPath . $module)) {
                $routesPath = $modulesPath . $module . '/Config/Routes.php';
                if (file_exists($routesPath)) {
                    require($routesPath);
                }
            }
        }
    }
    ```

3. **Mettre à jour l'autoloader dans app/Config/Autoload.php**

    Ajoutez un nouvel espace de noms pour les modules dans le tableau $psr4 :

    ```php
    public $psr4 = [
        'App' => APPPATH,
        APP_NAMESPACE => APPPATH,  // To ensure filters, etc still found
        'Config' => APPPATH . 'Config',
        'Modules' => ROOTPATH . 'modules',  // Ajout pour les modules
    ];
    ```

4. **Créer la structure du nouveau module "Users"**

    Structurez votre module "Users" comme suit :

    ```
    /modules
      /Users
        /Config
          Routes.php
        /Controllers
          Users.php
        /Models
        /Views
    ```

5. **Définir les routes dans modules/Users/Config/Routes.php**

    Ajoutez la route pour accéder à votre contrôleur Users :

    ```php
    $routes->get('users', '\Modules\Users\Controllers\Users::index');
    ```

6. **Créer le contrôleur dans modules/Users/Controllers/Users.php**

    Créez le fichier Users.php avec le contenu suivant :

    ```php
    <?php
    namespace Modules\Users\Controllers;
    use CodeIgniter\Controller;

    class Users extends Controller
    {
        public function index()
        {
            echo "Hello from the Users module!";
        }
    }
    ```

Avec ces étapes, vous avez configuré un module Users simple dans CodeIgniter 4, en utilisant une architecture modulaire qui facilite la gestion et l'extension de votre application. Vous pouvez maintenant accéder à votre module via l'URL /users pour voir le message "Hello from the Users module!".

[🇫🇷 Français](#fr) | [🇺🇸 English](#en) 
EN
---
# Tutorial: Creating a Modular System in CodeIgniter 4

This tutorial guides you through creating a modular system in a CodeIgniter 4 application, including setting up routes, autoload, and module-specific controllers.

## Steps

1. **Create the "modules" folder at the root of your CodeIgniter project**

    Organize your folders to include a new modules folder at the root of the project. The structure should look like this:

    ```
    /CodeIgniter
    |-- /app
    |-- /modules  # Folder for modules
    |-- /public
    |-- /system
    |-- /writable
    ```
2. **Modify the routes in app/Config/Routes.php**

    Add the following code to dynamically include the route configuration files for each module.

    ```php
    /**
     * ---------------------------------------------------------------
     * Include Modules Routes Files
     * ---------------------------------------------------------------
     */
    if (file_exists(ROOTPATH . 'modules')) {
        $modulesPath = ROOTPATH . 'modules/';
        $modules = scandir($modulesPath);

        foreach ($modules as $module) {
            if ($module === '.' || $module === '..') continue;
            if (is_dir($modulesPath . $module)) {
                $routesPath = $modulesPath . $module . '/Config/Routes.php';
                if (file_exists($routesPath)) {
                    require($routesPath);
                }
            }
        }
    }
    ```

3. **Update the autoloader in app/Config/Autoload.php**

    Add a new namespace for the modules in the $psr4 array:

    ```php
    public $psr4 = [
        'App' => APPPATH,
        APP_NAMESPACE => APPPATH,  // To ensure filters, etc still found
        'Config' => APPPATH . 'Config',
        'Modules' => ROOTPATH . 'modules',  // Addition for modules
    ];
    ```

4. **Create the structure for the new "Users" module**

    Structure your "Users" module as follows:

    ```
    /modules
      /Users
        /Config
          Routes.php
        /Controllers
          Users.php
        /Models
        /Views
    ```

5. **Define the routes in modules/Users/Config/Routes.php**

    Add the route to access your Users controller:

    ```php
    $routes->get('users', '\Modules\Users\Controllers\Users::index');
    ```

6. **Create the controller in modules/Users/Controllers/Users.php**

    Create the Users.php file with the following content:

    ```php
    <?php
    namespace Modules\Users\Controllers;
    use CodeIgniter\Controller;

    class Users extends Controller
    {
        public function index()
        {
            echo "Hello from the Users module!";
        }
    }
    ```

With these steps, you have configured a simple Users module in CodeIgniter 4, using a modular architecture that makes managing and extending your application easier. You can now access your module via the /users URL to see the "Hello from the Users module!" message.
