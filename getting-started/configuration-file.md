# საკონფიგურაციო ფაილი
<p class="uk-article-lead">შესაძლებელია შეიძლვალოს პარაეტრები Pagekit-ის საკონფიგურაციო ფაილში.</p>

Pagekit-ის საკონფიგურაციო ნაწილები მოავსებულია ფაილში `/config.php`, რომელიც განთავსებულია ძირეულ პაპკაში. მის ავტოგენერაცი ხდება ინსტალაციის დროს. ნორმალურ სიტუაციაში მისი რედაქტირება ხდება  Pagekit-ის _System > Settings_ ადმინისტრატორის არიდან, მაგრამ არსებობს სიტუაციები, როდესაც საჭირო ხდება მისი ხელით რედაქტირებაც. მოყვანილი ნაწილები განსაზღვრავს ხშირიდ საჭირო პარამეტრებს.

```php
'database' => [
  'default' => 'mysql',      // ძირეული მონაცემთა ბაზა
  'connections' => [         // მასივი მონაცემთა ბაზასთან შეერთებისათვის
    'mysql' => [             // მონაცემთა ბაზის დრაივერის სახელი, mysql ან sqlite
      'host' => 'localhost', // მონაცემთა ბაზის სერვერის სახელი
      'user' => 'user',      // მონაცეთა ბაზის მომხმარებლის სახელი
      'password' => 'pass',  // მონაცეთა ბაზის პაროლი
      'dbname' => 'pagekit', // მონაცემთა ბაზის სახელი
      'prefix' => 'pk_'      // ცხრილების პრეფიქსი
    ]
  ]
],
'system' => [
  'secret' => 'secret'       // საიდუმლო სტრიქონი, რომელიც გენერიდება ინსტალაციის დროს
],
'system/cache' => [
  'caches' => [
    'cache' => [
      'storage' => 'auto'    // თუ ჩართულია, კეშირების მეთოდი.
    ]
  ],
  'nocache' => false         // კეშის მდგომარეობა - თქვენ შეგიძლიათ გათიშოთ, თუ მის მნიშვნელობას დააყენებთ  'true'-ს
],
'system/finder' => [
  'storage' => '/storage'    // ფართობითი გზა საწყობის პაპკისკენ, სადაც მედია ფაილები უნდა იქნას შენახული
],
'application' => [
  'debug' => false           // debug რეჟიმის მდგომარეობა - ჩართავთ საითის პროგრამირებისას
],
'debug' => [
  'enabled' => false         //  debug- ინსტრუმენტეის ზოლის მდგომარეობა
]
```
