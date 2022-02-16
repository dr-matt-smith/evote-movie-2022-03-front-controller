# evote-movie-2022-03-front-controller

part of the evote-2022 project sequence

- [https://github.com/dr-matt-smith/evote-movie-2022](https://github.com/dr-matt-smith/evote-movie-2022)


## NOTES for this project step

Add Front Controller PHP OO architecture

- move all display pages into folder `/templates`
 
  - (keep) images and css in folder `/public`

- create `src` folder (for all our class files)

- create PHP class `/src/MainController.php` class with methods to display each of the page templates

    ```php
    <?php
    
    namespace Tudublin;
    
    class MainController
    {
        public function index()
        {
            require_once __DIR__ . '/../templates/index.php';
        }
    
        public function about()
        {
            require_once __DIR__ . '/../templates/about.php';
        }
    
        public function contact()
        {
            require_once __DIR__ . '/../templates/contact.php';
        }
    
        public function list()
        {
            require_once __DIR__ . '/../templates/list.php';
        }
    
        public function sitemap()
        {
            require_once __DIR__ . '/../templates/sitemap.php';
        }
    }
    ```

- create `composer.json` to define PHP namespaced `Tudublin` classes in `/src`:

    ```json
    {
      "autoload": {
        "psr-4": {
          "Tudublin\\": "src"
        }
      }
    }
    ```
 
- at the terminal command line run `composer dump-autoload` to create a `/vendor` folder and containing autoloading script `autoload.php`

- add root website script `/public/index.php` to run the autoload,  create an `Application` object and invoke its `run()` method:

    ```php
    <?php
    require_once __DIR__ . '/../vendor/autoload.php';
    
    use Tudublin\Application;
    $app = new Application();
    $app->run();
    ```

- in the `/templates` folder change **all** navigation links in the HTML content to the form `/?action=<PAGE>`. This means that **EVERY** request goes through `/public/incdexphp` and our application's switch-statement in `Application::run()`

  - e.g. `/?action=about` for link to about page
  
  - e.g. `/?action=sitemap` for link to sitemap page
  
 
- create class `/src/Application.php` with a `run()`  method to test for value of `GET` variable `action`, create a `$mainController` object, and invoke the `MainController` method that corresponds to the value found for `action` in the URL

    ```php
    <?php
    
    namespace Tudublin;
    
    class Application
    {
        public function run()
        {
            $action = filter_input(INPUT_GET, 'action');
            $mainController = new MainController();
    
            switch ($action){
                case 'about':
                    $mainController->about();
                    break;
    
                case 'contact':
                    $mainController->contact();
                    break;
    
                case 'sitemap':
                    $mainController->sitemap();
                    break;
    
                case 'list':
                    $mainController->list();
                    break;
    
                default:
                    $mainController->index();
            }
        }
    }
    ```