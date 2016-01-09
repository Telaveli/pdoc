# მარშრუტიზაცია
<p class="uk-article-lead">Pagekit-ის მიერ გადაწყვეტილი ცენტრალური ამოცანა არის _Routing_. როდესაც ბროუზერი დაუკვეთავს URL-ს, ფრეიმვორკი გადაწყვეტს რომელ აქტივობას უნდა გამოუძახოს.</p>

## კონტროლერი
 Pagekit-ში მარშრუტების შესაქმნელად ყველაზე გავრცელებული ხერხია   _კონტროლერის_შექმნა.  _კონტროლერი_ მგრძნობიარეა ხელით შეკვეთაზე, პარამეტრებში მითითებულ მარშრუტებზე და ინტერფეისის წარმოდგენაზე.

### კონტროლერის რეგისტრაცია
 _კონტროლერის_ რეგისტრაცაი შესაძლებელია საკუთარ [მოდულის კონფიგურაციაში](modules.md). უნდა გამოიყენოთ  `routes` თვისება, იმისათვის რომ კონტროლერი მიაბათ მარშრუტზე.

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

ძირეულად, თქვენი გაფართოება (ან თემა) დაიბუთება და მითითებული ძირეული მარშრუტების გენერაცია მოხდება ავტომატურად.  [developer toolbar-ის](../tools/developer-toolbar.md) გამოყენებით შესაძლებელია ნახოთ ყველა ახლად შექმნილი მარშრუტი (ყველა სხვა ძირითად მარშრუტებთან ერთად).

ქვემოთ მოცემულია, თუ როგორ უნდა გაიგოთ მარშრუტი:

მარშრუტი                                                                   | აღწერა
------------------------------------------------------------------------ | -----------------------------------------------------------------------
Name <br> `@hello/hello/settings`                                        | მარშრუტის სახელი, უნდა გამოიყენოთ მარშრუტის შესაქმნელად (უნდა იყოს უნიკალური).
URI <br> `/hello/settings`                                               | ამ მარშრუტზე ბროუზერიდან დასაშვები გზა.
Action <br> `Pagekit\Hello\Controller\DefaultController::settingsAction` | კონტროლერის ფუნქცია, რომელიც უნდა იქნას გამოძახებული.

ძირითადად, მარშრუტებს აქვთ შემდეგი ფორმა  `http://example.com/<extension>/<controller>/<action>`. სპეციალური აქცია არის  `indexAction` რომელიც არ ებმება, როგორც `.../index`  მხოლოდ, როგორც `.../`. დამატებითი ოპციები საკუთარი მარშრუტებისათვის ხელმისაწდომია კურსის სახით, რომელსაც იხილავთ მომდევნო აღწერაში.

**შენიშვნა**: ასახელებული მარშრუტები უნდა იყოს უნიკალური. წინააღმდეგ შემთხვევაში სისტემა დაიმატებს პირვალად ასახელებულ მარშრუტს, მაგრამ შემდგომი სისტემის მოქცევა არ იქნება პროგნოზირებადი, ამიტომ სჯობს, დარწმუნდეთ მარშრუტების უნიკალურობაში.

### ანოტაციები
კონტრელერის ბევრი ქცევა განისაზღვრება კლასებში და მეთოდებში მოყვანილი ინფორმაციის საფუძველზე. აქ არის მოკლე მიმოხილვა ანოტაციების, რომელთა სრული განსაზღვრება მოყვანილი ქვემოთ.

ანოტაცია   | აწერა
---------- | -------------------------------------------------------------
`@Route`   | მარშრუტი აქციის ან მთციანი კონტროლერის მისაბმელად.
`@Request` | Handle parameter passing from the http request to the method.
`@Access`  | ამოწმებს მომხმარებლის დაშვების უფლებას.

#### @Route
განსაზღვრავს სად მიებმება კონტროლერი ან მისი აქცია. შესაძლებელია ანოტაცია კლასების და მეთოდების აღწერისას.

ძირეულად, მეთოდი სახელით  `greetAction` უნდა მოებას, როგორც  `/greet` კლასის მარშრუტზე. მომხმარებლის მარშრუტების დასამატებლად შესაძლებელია რამოდენიმე დამატებითი ჩანაწერის გამოყენება მეტოდზე მარშრუტის დასამატებლად. მასში შესაძლებელია აგრეთვე დინამიური პარამეტრის გამოყენება, როცელიც გადაეცემა მეთოდს.

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

პარამეტრი შესაძლოა იყოს გარკვეული სპეციფიკაციის, მაგალითად დააწესოს ლიმიტი რიცხვზე. თქვენ შეგიძლიათ აირჩიოთ სახელი ისე, რომ გქონდეთ წვდომა მასზე თქვენი კოდიდან. გამოიყენეთ Use PHP-ის არგუმენტის defaults მეთოდის აწერისას, იმისათვის რომ მიუთითოთ პარამეტრის ოპიცია.

მარშრუტები შესაძლებელია დაკავშირებული იყოს განსაზღვრულ Http-მეთოდებთან (ი.რ. `GET` ან `POST`). ეს განსაკუთრებით სასარგებლო იქნება  RESTful API'-თვის.

```php
/**
 * @Route("/view/{id}", name="@hello/view/id", requirements={"id"="\d+"}, methods="GET")
 */
public function viewAction($id = 0)
{
    // ...
}
```

**შენიშვნა** `@Route` ანოტაციაზე უფრო დეტალური ინფორმაციის და მაგალითების მიღება შეიძლება [Symfony's საკუთარ დოკუმენტაციაში](http://symfony.com/doc/current/bundles/SensioFrameworkExtraBundle/annotations/routing.html).

#### @Request
თქვენ შეგიძლიათ უჩვენოთ შეკვეთით გადაცემული მონაცემის ტიპები და შეუსაბამოთ ანოტირებულ მეთოდში გადაცემულ პარამეტრებს.
მასივის რუკაა _სახელი_ : _ტიპი_. _სახელი_ არის გასაღები შეკვეთაში მოყვანილი მონაცემების შიგნით. _ტიპი_ შეიძლება იყოს `int`, `string`, `array` და დამატებითი ტიპი advanced types like `int[]`-ს მსგავსი მთელი რიცხვების მასივისათვის. თუ ტიპი არ იქნება მითითებული, ნაგულისხმევი იქნება `string`.

პარამეტრობის რიგითობას ადგენს მეთოდის თავში წარმოდგენილი რიგითობა, სახელები შესაძლოა იყოს განსხვავებული.

```php
/**
 * @Request({"id": "int", "title", "config": "array"}, csrf=true)
 */
public function saveAction($id, $title, $config)
{
  // ...
}
```

შესაძლოა, ჩასვათ შემოწმება  [CSRF](http://en.wikipedia.org/wiki/Cross-site_request_forgery). დაამატეთ `csrf=true` თქვენ შეკვეთას და მიუერთე  `@token` ინტერფეისს, რომელიც ამოწმებს ამ მეთოდს.

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
