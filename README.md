[![pagepro_logo](https://git.freshmind.pl/uploads/-/system/project/avatar/124/pobrane.png?width=64)](https://printu.pl/)

# PHP Developer - Rekrutacja

## Zadanie 1
Twoje zadanie to napisanie funkcji haszującej, służącej do ukrywania rosnących numerów zamówień. 

```php
public function hashOrder(int $number): string
```

### Wymagania
Funkcja ma uniemożliwić odgadnięcie oryginalnego numeru zamówienia, w przypadku w którym nie posiadamy wyników dla koolejnych numerów zamówień. Np dla zamówienie `100`, `350`, `567`.

 - fukncja przyjmuje na wejściu numer zamówienia w postaci `int`
 - funkcja zwraca `string` o długości 7 znaków
 - jedyne dozwolone znaki to `0123456789`
 - wielokrotne wywołane metody z tymi samymi danymi wejściowymi powinno zwrócić ten sam wynik
 - w przedziale `1..9999999` funkcja musi zwrócić unikalny `string`

### Przykład

```php
> echo hashOrder(100)
> "8002927"
> echo hashOrder(350)
> "3010247"
> echo hashOrder(567)
> "6276600"
```

### Technologie
 - PHP >=7.4
 - bcmath

### Rozwiązanie

Funkcja zostanie przetestowana następującym skryptem.

```php
$unique = [];

echo "Starting test ....".PHP_EOL;
$start = microtime(true);

for ($i=1; $i<=9999999; $i++) {
  $result = hashOrder($i);

  if (!preg_match("/^[0-9]{7}$/", $result)) {
    throw new InvalidArgumentException("Result {$result} does not match regex");
  }

  if (!empty($unique[$result])) {
    throw new InvalidArgumentException("Colision detected for key {$i}:{$unique[$result]} and result {$result}");
  }

  $unique[$result] = $i;
}

$time_elapsed_secs = microtime(true) - $start;

if ($time_elapsed_secs > 60) {
  throw new InvalidArgumentException("Could not finish test in 60 seconds");
}

echo "Finished in {$time_elapsed_secs}";
```

## Zadanie 2
Znajdź unikalny znak. Unikalny znak to taki, który występuje tylko jeden raz w `string`. Biorąc pod uwagę ciąg zawierający małe litery angielskie, zwróć indeks pierwszego wystąpienia unikalnego znaku w ciągu, używając indeksowania opartego na 1. Jeśli łańcuch nie zawiera żadnych unikalnych znaków, zwróć -1

```php
public function findUniqueString(string $s): int
```

### Przykład 
`s = "statistics"`

Unikalne znaki to `[a, c]` z których `a` występuje pierwsze. Wg indexu zaczynającego się od `1`, znak `a` znajduje się na `3` pozycji.

### Ograniczenia
 - 1 <= length(s) <= 10
 - `s` zawiera tylko małe litery angielskie


### Rozwiązanie

```php
> echo findUniqueString('hashthegame');
> 3
```

## Zadanie 3
Przygotuj schmat [OpenAPI 3.0](https://swagger.io/specification/) (json lub yaml) dla następującej końcówki API. 

### Wymagania

> POST /v1/upload

Wymagane nagłówki
> Autorization: Bearer xxxx  
> Content-Type: application/json  
>
Request body:
```json
[
  {
    "part": 1,
    "etag": "a54357aff0632cce46d942af68356b38"
  },
  {
    "part": 2,
    "etag": "0c78aef83f66abc1fa1e8477f296d394"
  },
  {
    "part": 3,
    "etag": "acbd18db4cc2f85cedef654fccc4a4d8"
  }
]
```

Response 200 OK:
```json
{
  "id": 2058959,
  "timestamp": 1516354800,
  "source": "instagram",
  "folder": null,
  "height": 1080,
  "width": 1080,
  "mime": "image/jpeg",
  "display": {
    "thumbnail": {
      "width": 200,
      "height": 200,
      "src": "https://printu.test/media/catalog/cache/vH72u0x7Y4uf0Tdd1j%252F34EFgAluLOcapxlHI0rcqWYDEZi8ph6XztIGW4VGE1cuYIyIE/image.jpg"
    },
    "preview": {
      "width": 800,
      "height": 800,
      "src": "https://printu.test/media/catalog/cache/vH72u0x7Y4uf0Tdd1j%252F34EFgAluLOcapxlHI0rcqWYDEZi8ph6XztIsI%252BRE60RFRtAsn/image.jpg"
    },
    "square": {
      "width": 200,
      "height": 200,
      "src": "https://printu.test/media/catalog/cache/vH72u0x7Y4uf0Tdd1j%252F34EFgAluLOcapxlHI0rcqWYDEZi8ph6XztIsI%252BRE60RFRtAsn/image.jpg"
    },
    "original": {
      "width": 1080,
      "height": 1080,
      "src": "https://dev-printu-net.s3.eu-central-1.amazonaws.com/photobook/12/7/0/2058959.jpg?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAILKHTK5ZOAVQB23Q%2F20180209%2Feu-central-1%2Fs3%2Faws4_request&X-Amz-Date=20180209T092316Z&X-Amz-SignedHeaders=host&X-Amz-Expires=518400&X-Amz-Signature=cc95d836e2235992e1948add1bc8f5cdb35d39dc974f8cfa23efd9895db32d38"
    }
  },
  "metadata": {
    "gps": "52/1, 15/1, 272/100 N 21/1, 1/1, 5851/100 E",
    "datetime": 1516354800,
    "name": "IMG_9171.JPG"
  }
}
```

Response >=400 <=599:
```json
{
  "errorCode": 400,
  "userMessage": "The request is missing a required parameter, includes an invalid parameter value, includes a parameter more than once, or is otherwise malformed.",
  "devMessage": "Check the client_id parameter.",
  "more": "",
  "applicationCode": "X1234"
}
```

### Rozwiązanie

Przygotowany schemat zostanie przetestowany przy pomocy [openapi-spec-validator](https://github.com/python-openapi/openapi-spec-validator)

```bash
openapi-spec-validator --schema 3.0.0 openapi.yml
```


## Podsumowanie

Wykonane zadanie prosze umieścić na publicznym repozytorium. np `github`, `gitlab`.