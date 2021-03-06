# inpost-api-php-sdk

Biblioteka PHP do obsługi API usług Inpost (https://b2b.inpost.pl/pl/e-commerce/jak-sie-zintegrowac)

Aktualnie obsługiwana wersja API to **2.1.11**
## Stabilność
[![GitHub version](https://badge.fury.io/gh/t3ko%2Finpost-api-php-sdk.svg)](https://badge.fury.io/gh/t3ko%2Finpost-api-php-sdk)

**Aktualnie dostępna tylko wersja wczesno-deweloperska. Brak wersji stabilnej/produkcyjnej.**

## Instalacja
Najprostszy sposób to instalacja za pomocą Composer-a (http://getcomposer.org).

Poprzez plik `composer.json`:
```
"require": {
    "t3ko/inpost-api-php-sdk": "dev-master"
},
```
lub z linii poleceń:
```
composer require t3ko/inpost-api-php-sdk
```

## Użycie

Najprostszy możliwy przykład:
```
<?php

require_once __DIR__.'/vendor/autoload.php';

$api = new T3ko\Inpost\Api\Client(
    'test@testowy.pl',
    'WqJevQy*X7',
    \T3ko\Inpost\Api\Client::SANDBOX_API_ENDPOINT);

$machinesList = $api->getMachinesList();

```
Utworzenie, rejestracja i potwierdzenie(opłacenie) nowej paczki:
```
$package = (new \T3ko\Inpost\Objects\Shipment\PackageBuilder(
        'test@testowy.pl',
        \T3ko\Inpost\Objects\Shipment\Size::A,
        'odbiorca@exmple.org',
        '501500500',
        'BBI005'))->build();
        
$registerResponse = $api->registerShipment($package);

var_dump($registerResponse);
/*
array(2) {
  ["packcode"]=>
  string(24) "622222209743346017700007"
  ["calculatedcharge"]=>
  string(4) "1.00"
}
*/

$api->confirmShipment($registerResponse['packcode']);
```

Pobranie PDF z etykietą opłaconej paczki:
```
$fileContents = $api->getSticker('622222209743346017700007', \T3ko\Inpost\Api\Client::LABEL_SIZE_A6);

$fp = fopen('622222209743346017700007.pdf', 'wb+');
fwrite($fp, $fileContents);
```

## Dokumentacja
Dokumentacja funkcjonalna API Inpost dostępna poniżej:

https://b2b.paczkomaty.pl/pl/strefa-kilenta/informacje-dla-klientow/dokumentacja-api-paczkomatow-inpost
## Licencja

MIT License

Copyright (c) 2016 Tomasz Konarski

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.