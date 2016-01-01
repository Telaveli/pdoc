# ვიჯეტები
<p class="uk-article-lead">ვიჯეტების გამოყნებით შესაძლებელია საიტის სხვადასხვა ადგილას შესრულებული იქნას კონტენტის პატარა ნაჭრები.</p>

იმის განსაზღვრისავის, თუ სად უნდა განთავსდეს ვიჯეტის კონტენტი საიტზე, სამართვავ პანელში არის _Widgets_ განყოფილება, იმისათვის, რომ შესაძლებელი იყოს ვიჯეტის გამოქვეყნება სასურველ პოზიციაში, რომლებიც განსაზღვრულია თემის მიერ. გაფართოებაშიც და თემებაშიც ვიჯეტი მოსულია ერთნაირად რაიმე პროგრამული განსხვავების გარეშე.

## ვიჯეტის პოზიციის განსაზღვრა თქვენს თემაში
თქვენ შეგიძლიათ რამდენიც საჭიროა იმდენი პოზიცია განსაზღვროთ ვიჯეტისათვის თქვენი თემის `index.php` ფაილში.

```php
'positions' => [

    'sidebar' => 'Sidebar',
    'footer' => 'Footer',

],
```

## თემაში ვიჯეტის რენდერი
იმისათვის რომ დაარენდეროთ ვიჯეტი ყველგან გამოქვეყნებულ პოზიციაზე, შესაძლებელია გამოიყენოთ რენდერის ეგზეპლიარი თქვენი თემის  `views/template.php`შაბლონიდან:

```php
<?php if ($view->position()->exists('sidebar')) : ?>
    <?= $view->position('sidebar') ?>
<?php endif; ?>
```

## ვიჯეტის ახალი ტიპის რეგისტრაცია
ვიჯეტის ახალი ტიპის რეგისტრაციისათვის შეგიძლიან გამოიყენოთ `widgets` თვისება თქვენ `index.php` ფაილში.

```php
'widgets' => [

    'widgets/hellowidget.php'

],
```

## ვიჯეტის ახალი ტიპის განსაზღვრა
შინაგანად, ვიჯეტი Pagekit-ში მოდულია. ამიტომ ის განსაზღვრულია მოდულის განსაზღვრებით: A PHP მასივი განსაზღვრული თვისებების ნაკრებით.

`widgets/hellowidget.php`:

```php
<?php

return [

    'name' => 'hello/hellowidget',

    'label' => 'Hello Widget',

    'events' => [

        'view.scripts' => function ($event, $scripts) use ($app) {
            $scripts->register('widget-hellowidget', 'hello:js/widget.js', ['~widgets']);
        }

    ],

    'render' => function ($widget) use ($app) {

        // ...

        return $app->view('hello/widget.php');
    }

];
```

ეს მაგალში დამატეით შემოდის JS კომპონენტი განთავსებული `hello:js/widget.js`და php ხედის ფაილი `hello/widgets/helloview.php`რომელიც გამოიყენება ინტერფეისში ვიჯეტის გამოსახვისავის.

`js/widget.js`:

````
window.Widgets.components['system-login:settings'] = {

    section: {
        label: 'Settings'
    },

    template: '<div>Your form markup here</div>',

    props: ['widget', 'config', 'form']

};

`views/widget.php`:

```php
<p>Hello Widget output.</p>
````

**შენიშვნა** კარგი მაგალითი სრული ვიჯეტის კარგი მაგალითი განთავსებულია მისამართზე `app/system/modules/user/widgets/login.php`  Pagekit-ის ძირში.
