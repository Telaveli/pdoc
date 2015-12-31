# მოდულები
<p class="uk-article-lead">აპლიკაციების პროგრამული კოდისათვის Pagekit-ი იყენებს *მოდულებს*. მოდულის აგებისას გამოიყენება პირველადი ჩატვირთვის, მარშრუტიზაციის და სხვა საკონფიგურაციო ოპციები. აქ თქვენ შეგიძლიათ დაწეროთ მოვლენები, დაამატოთ საკუთარი კლასები და კონტროლერები.</p>

## განსაზღვრება: index.php
იმისათვის, რომ ჩატვირთოს ან დააკონფიგურიროს მოდული, Pagekit-ს აქვს მენეჯერი -  ModuleManager. ის უყურებს  `index.php` ფაილს მოდულის ძირეულ დირექტორიაში და ელოდება მისგან  PHP მასივის დაბრუნებას. ჩათვალეთ, რომ მოდულის ჩავირთვიათვის ეს მასივი უპირველესია.

ამ მასივში თვისებების სწორად დაწერით, თქვენ ეუბნებით Pagekit-ს ყველაფერს, თუ რა უნდა იცოდეს ამ მოდულის შესახებ.

```php
<?php

/*
 * აბრუნებს მოდულის განმსაზღვრელ მასივს.
 */
return [

    // Required: Unique module name
    'name' => 'hello',

];
```

ეს არის მოდულის სწორად აღწერის მინიმალური მაგალითი, მაგრამ ის არაფერს არ აკეთებ, გარდა იმისა რომ ჩატვირთული იქნება  Pagekit-ის მიერ. მოდული იტვირთება მხოლოდ მაშინ, თუ ის ჩართულია ატმინისტრატორის მართვის პანელიდან.

**შენიშვნა:** თუ გადახედავთ If you start exploring Pagekit-ის შინაგან სტრუქტურას, ასეთ მოდულის კონსტრუქციას თქვენ მრავალ ადგილას შეხდებით, ესაა Pagekit-ის არქიტექტურის ძირითადი კონცეფცია.

### **main**: Bootstrap (პირველი ჩატვირვის) კოდი
Theფუნქცია `main` იძახება, როცა იტვირთება მოდული. მას პარამეტრის სახით გადაეცემა  Pagekit-ის აპლიკაციის კონტეინერის ეგზეპლიარი.

```php
use Pagekit\Application;

// ...

'main' => function (Application $app) {
    // ჩასატვირთი კოდი
}
```

თქვენ ასევე, შეგიძლიათ ჩატვითოთ მოდულის კლასი. საკუთარი სახელთა სივრცის ჩატვითვა აუცილებელია მისი მუშაობისათვის. (იხ. `autoload` თვისება). აგრეთვე საჭიროა, რომ კლასმა გააკეთოს `Pagekit\Module\ModuleInterface` რეალიზება.

```php
'main' => 'MyNamespace\\MyExtension',
```

### **autoload**: საკუთარი სახელთა სივრცის რეგისტრაცია
Pagekit-ის მიერ მოხდება ავტომატური ჩატვირთვა გადაეცემული სახელების და გზების სიისა. არსებული კლასები ხელმისაწვდომი უნდა იყოს ავტოჩამტვირთველისათვის (`use Pagekit\Hello\HelloExtension`). ფაილების განთავსების გზები ეფარდება მოდულის განთავსებას.

```php
'autoload' => [

    'Pagekit\\Hello\\' => 'src'

]
```

### **routes**: კონტროლერების მიერთებაMount controllers
Use the `routes` თვისების გამოყენებით ხდება კონტროლერების მასშუტზე მიერთება. წაიკითხეთ მეტი ამის შესახებ  [მარშრუტიზაცია და კონტროლერები](routing.md).

```php
'routes' => [

    '/hello' => [
        'name' => '@hello/admin',
        'controller' => [
            'Pagekit\\Hello\\Controller\\HelloController'
        ]
    ]

]
```

### **permissions**: დაშვებების განსაზღვრა
თქვენს მოდულს შეუძლია განსაზღვროს დაშვებები. მათი მართვა შესაძლებელია  Pagekit User & Permissions არედან. თქვენ შეგიძლიათ დაიცვათ თქვენი მარშუტები ან დააწესოთ გარკვეული არასანქცირებული მოქმედებების აკრძალვა მომხმარებლების მრიდან.

```php
'permissions' => [

    'hello: manage settings' => [
        'title' => 'Manage settings'
    ]

]
```

### **resources**:  shorthand - რესურსების რეგისტრაცია
შეიძლება პრეფიქსების რეგირტრაცი, რამელიც გამოიყენება, როგორც მოკლე ვერსია ფაილების სრული გზისა. მაგალითისთვის გამოიყენება `views:admin/settings.php` რათა მივწვდეთ `packages/VENDOR/PACKAGE/views/admin/settings.php`ფაილს. Pagekit-ს თავის მხრივ, გაფართოებებისათვის და თემებისათვის,  უკვე რეგისტრირებული აქვს რამოდენიმე გზა.

ეს მუშაობს, როცა იყენებთ Pagekit-ის ფაილურ სისტემას(ე. ი. როცა ხდება ფაილის გზისთვის  url-ის ან კონტროლერიდან რომელიმე ხედის დარენდერების გენერაცია).

```php
'resources' => [

    'views:' => 'views'

],
```

### **events**: Pagekit-ის ან სხვა მოდულების მოვლენების მოსმენა
მოვლენები გამოიძახება Pagekit-ის ბირთვის რამოდენიმე წერტილში და პოტენციურად სხვა მოდულების მიერ. მოვლენას ყოველთვის აქვს მისი მაიდენტიფიცირებელი უნიკალური სახელი. შესაძლებელია უკუგამოძახების ფუნქციის რეგისტრაციაც.

For more information on the Event system, check out the [Events section](../developer-basics/architecture-events.md)

```php
'events' => [

    'view.scripts' => function ($event, $scripts) {
        $scripts->register('hello-settings', 'hello:app/bundle/settings.js', '~extensions');
    }

]
```

### **config**: მოტულის ძირითადი კონფიგურიაცია
ესენია მოტდულის ძირითადი საკონფიგურაციო ცვლადები.

```php
'config' => [
    'default' => 'World'
],
```

#### config-ის წაკითხვა
მოდულის კონფიგურაციის წაკითხვა შესაძლებელია `config` თვისებით module-ს ეგზეპლიარიდან. ეს არის შედეგი ორი config-ის  is the result of both the default config stored inside the `index.php` და ც changes that are stored in the database.

```php
$config = $app->module('hello')->config;
```

#### Write config
To store changes for a module config, use the `config()` service. These changes will automatically propagate to the database.

```php
// Complete config
$app->config()->set('hello', $config);

// Single Value
$app->config('hello')->set('message', 'Custom message');
```

**Note**. If you directly read the config from the module, it will still have the old value. After the next request, Pagekit will have merged the changes and made them available as the `config` property of the `$module` instance.

### **nodes**: Register Nodes for the Site Tree
Nodes are similar to routes with the main difference that they can be dragged around in the Site Tree View and therefore dynamically result in a calculated route.

When you have added a Node, it will be available in the Site Tree. Click the _Add Page_ button to see the Dropdown of all available Node types.

For more information on nodes, check out the [Routing section](../developer-basics/routing.md

```php
'nodes' => [

    'hello' => [

        // The name of the node route
        'name' => '@hello',

        // Label to display in the admin panel
        'label' => 'Hello',

        // The controller for this node. Each controller action will be mounted
        'controller' => 'Pagekit\\Hello\\Controller\\SiteController'
    ]

]
```

### **menu**: Add menu items to the admin panel
You can add menu items to the admin panel's main navigation. These can link to any registered route and be limited to certain access permissions. The `access` property determines if the menu item is visible or not.

```php
'menu' => [

        // name, can be used for menu hierarchy
        'hello' => [

            // Label to display
            'label' => 'Hello',

            // Icon to display
            'icon' => 'hello:icon.svg',

            // URL this menu item links to
            'url' => '@hello/admin',

            // Optional: Expression to check if menu item is active on current url
            // 'active' => '@hello*'

            // Optional: Limit access to roles which have specific permission assigned
            // 'access' => 'hello: manage hellos'
        ],

        'hello: panel' => [

            // Parent menu item, makes this appear on 2nd level
            'parent' => 'hello',

            // See above
            'label' => 'Hello',
            'icon' => 'hello:icon.svg',
            'url' => '@hello/admin'
            // 'access' => 'hello: manage hellos'
        ]

    ],
```

### **settings**: Link to a settings screen
Link to a route that renders your settings screen. Setting this property makes Pagekit render a _Settings_ button next to your theme or extension in the admin panel listing.

```php
'settings' => '@hello/admin/settings',
```

### **widgets**: Register Widgets
A Widget is also a module. With the `widgets` property you can register all widget module definition files. Each of those files is expected to return a PHP array in the form of a valid module definition. Learn more about [Widgets](../developer-guides/widgets.md).

```php
'widgets' => [

    'widgets/form.php'

],
```

## Module config

```
TODO
```
