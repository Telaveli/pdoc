# მარშრუტიზაცია
<p class="uk-article-lead">Pagekit-ის მიერ გადაწყვეტილი ცენტრალური ამოცანა არის _Routing_. როდესაც ბროუზერი დაუკვეთავს URL-ს, ფრეიმვორკი გადაწყვეტს რომელ აქტივობას უნდა გამოუძახოს.</p>

## კონტროლერი
 The most common way in Pagekit-ში მარშრუტების შესაქმნელად ყველაზე გავრცელებული ხერხია   _Controller_-ის განსაზღვრა.  _Controller_ მგრძნობიარეა ხელით შეკვეთაზე, პარამეტრებში მითითებულ მარშრუტებზე და ინტერფეისის წარმოდგენაზე.

### კონტროლერის რეგისტრაცია
 _Controller_ -ის რეგისტრაცაი შესაძლებელია საკუთარი [მოდულის კონფიგურაციაში](modules.md). უნდა გამოიყენოთ  `routes` თვისება, იმისათვის რომ კონტროლერი მიაბათ მარშრუტზე.

```php
'routes' => [

    '/hello' => [
        'name' => '@hello/admin',
        'controller' => [
            'Pagekit\\Hello\\Controller\\HelloController'
        ]
    ]

],
```

### საბაზო სტრუქტურა
კლასის ანოტაცია ხდება  `@Route("/hello")`, კონტროლერი _მოებმება_  `http://example.com/hello/` მისამართზე, ის რეაგირებას გააკთებს ყველა შეკვეთაზე, რაც კი იქნება ამ URL-ზე ან sub-URL-ზე, მაგალითად `http://example.com/hello/settings`.

```php
namespace Pagekit\Hello\Controller;

/**
 * @Route("/hello")
 */
class HelloController
{

    public function indexAction()
    {
        // ...
    }

    public function settingsAction()
    {
        // ...
    }
}
```

ძირეულად, თქვენი გაფართოება (ან თემა) დაიბუთება და მითითებული ძირეული მარშრუტების გენერაცია მოხდება ავტომატურად. You can use the [developer toolbar-ის](../tools/developer-toolbar.md) გამოყენებით შესაძლებელია ნახოთ ყველა ახლად შექმნილი მარშრუტი (ყველა სხვა ძირითად მარშრუტებთან ერთად).

ქვემოთ მოცემულია, თუ როგორ უნდა გაიგოთ მარშრუტი:

მარშრუტი                                                                   | აღწერა
------------------------------------------------------------------------ | -----------------------------------------------------------------------
Name <br> `@hello/hello/settings`                                        | მარშრუტის სახელი, უნდა გამოიყენოთ მარშრუტის შესაქმნელად (უნდა იყოს უნიკალური).
URI <br> `/hello/settings`                                               | ამ მარშრუტზე ბროუზერიდან დასაშვები გზა.
Action <br> `Pagekit\Hello\Controller\DefaultController::settingsAction` | კონტროლერის ფუნქცია, რომელიც უნდა იქნას გამოძახებული.

By default, routes will be of the form `http://example.com/<extension>/<controller>/<action>`. A special action is `indexAction` which will not be mounted at `.../index`, but at `.../`. Advanced options for custom routes are available of course, as you will see in the next sections.

**Note**: If a route is not unique across your application, the one that has been added first is the one being used. As this is framework internal behavior that might change, you should not rely on this but rather make sure your routes are unique.

### Annotations
A lot of the controller's behavior is determined by information annotated to the class and methods. Here is a quick overview, a detailed description for each annotation follows below.

Annotation | Description
---------- | -------------------------------------------------------------
`@Route`   | Route to mount an action or the whole controller.
`@Request` | Handle parameter passing from the http request to the method.
`@Access`  | Check for user permissions.

#### @Route
Define the route the controller (or controller action) will be mounted at. Can be annotated to class and method definitions.

By default, a method called `greetAction` will be mounted as `/greet` under the class' route. To add custom routes, you can add any number of additional routes to a method. Routes can also include dynamic parameters which will be passed to the method.

```php
/**
 * @Route("/greet", name="@hello/greet/world")
 * @Route("/greet/{name}", name="@hello/greet/name")
 */
public function greetAction($name = 'World')
{
    // ...
}
```

Parameters can be specified to fulfill certain requirements (for example limit the value to numbers). You can name a route so that you can reference from your code. Use PHP argument defaults at the method definition to make a parameter optional.

Routes can be bound to certain Http-methods (e.g. `GET` or `POST`). This is especially useful for RESTful API's.

```php
/**
 * @Route("/view/{id}", name="@hello/view/id", requirements={"id"="\d+"}, methods="GET")
 */
public function viewAction($id = 0)
{
    // ...
}
```

**Note** For detailed information and more example, have a look at [Symfony's own documentation](http://symfony.com/doc/current/bundles/SensioFrameworkExtraBundle/annotations/routing.html) on the `@Route` annotation.

#### @Request
You can specify the types of data passed via a request and match them to parameters passed to the annotated method.

The array maps _name_ to _type_. _name_ is the key inside the request data. _type_ can be `int`, `string`, `array` and advanced types like `int[]` for an array of integers. If not type is specified, `string` is assumed by default.

The order of the keys will define the order in which parameters are being passed to the method. The parameter name in the method head can be anything.

```php
/**
 * @Request({"id": "int", "title", "config": "array"}, csrf=true)
 */
public function saveAction($id, $title, $config)
{
  // ...
}
```

You can also check for a token to protect against [CSRF](http://en.wikipedia.org/wiki/Cross-site_request_forgery). Add `csrf=true` to your request annotation and include the `@token` call in the view that submits a form to this method.

Checkout `Pagekit\Filter\FilterManager` for a complete list over available filters.

Some filters have additional options, like `pregreplace`:

```php
/**
 * @Request({"folders":"pregreplace[]"}, options={"folders" = {"pattern":"/[^a-z0-9_-]/i"}})
 */
public function deleteFolders($folders)
{
  // ...
}
```

#### @Access
You can specify certain user permissions required to access a specific method or the whole controller.

Controllers should always be specific for the frontend or the admin panel. So far we have seen controllers for the frontend. A administration controller will only be accessible for users with the `admin area access permission. Also, all routes for that controller will have a leading`admin/` in the URL. As a result, views will also render in the admin layout and not in the default theme layout.

```php
/**
 * @Access(admin=true)
 */
class SettingsController
{
  // ...
}
```

Now, only users with the admin area access permission can access the controller actions. If you want to use further restrictions and only allow certain users to do specific actions (like manage users etc.) you can add restrictions to single controller actions.

Define permissions in the `extension.php` (or `theme.php`) and combine them however you want. Access restrictions from the controller level will be combined with access restrictions on the single actions. Therefore you can set a basic _minimum_ access level for your controller and limit certain actions (like administrative actions) to users with more specific permissions.

```php
/**
  * @Access("hello: manage users")
  */
  public function saveAction()
  {
    // ...
  }
```

Of course, you can also use these restrictions even if the controller is no admin area controller. You can also check for admin permissions on single controller actions.

```php
/**
  * @Access("hello: edit article", admin=true)
  */
  public function editAction()
  {
    // ...
  }
```

## Generating URLs
Using the URL service, you can generate URLs to your routes.

```php
$this['url']->route('@hello/default/index')          // '/hello/default/index'
$this['url']->route('@hello/default/index', true)    // 'http://example.com/hello/default/index'
$this['url']->route('@hello/view/id', ['id' => 23])  // '/hello/view/23'
```

## Links
Pagekit's routes can be described by an internal route syntax. They consist of the route name (e.g. '@hello/name'), optionally followed by GET parameters (e.g. '@hello/name?name=World'). This is called a link. It separates the actual route URI from the route itself.
