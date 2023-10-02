 
## Screenshot

![open-admin-reporter](https://user-images.githubusercontent.com/86517067/176226958-b3ed0a1c-7b87-4e43-a2fd-f487f110d9f5.png)


## Installation

```
$ composer require snowsoft/oareporter

$ php artisan vendor:publish --tag=open-admin-reporter

$ php artisan migrate --path=vendor/snowsoft/oareporter/database/migrations

$ php artisan admin:import reporter
```

Open `app/Exceptions/Handler.php`,
1) Add: `use OpenAdmin\Admin\Reporter\Reporter;`
2) Call `Reporter::report()` inside `register` ... `reportable` method:
```php
<?php

namespace App\Exceptions;

use Illuminate\Foundation\Exceptions\Handler as ExceptionHandler;
use OpenAdmin\Admin\Reporter\Reporter;
use Throwable;


class Handler extends ExceptionHandler
{
    /**
     * A list of the exception types that are not reported.
     *
     * @var array
     */
    protected $dontReport = [
        //
    ];

    /**
     * A list of the inputs that are never flashed for validation exceptions.
     *
     * @var array
     */
    protected $dontFlash = [
        'current_password',
        'password',
        'password_confirmation',
    ];

    /**
     * Register the exception handling callbacks for the application.
     *
     * @return void
     */
    public function register()
    {
        $this->reportable(function (Throwable $e) {
            // Add This line
            Reporter::report($e);
        });
    }
}

```

Open `http://localhost/admin/exceptions` to view exceptions.

License
------------
Licensed under [The MIT License (MIT)](LICENSE).
