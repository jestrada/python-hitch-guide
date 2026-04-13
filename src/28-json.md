# JSON

![image](https://docs.python-guide.org/_static/photos/33928819683_97b5c6a184_k_d.jpg)

The [json](https://docs.python.org/3/library/json.html) library can parse JSON from strings or files. The library parses JSON into a Python dictionary or list. It can also convert Python dictionaries or lists into JSON strings.

## Parsing JSON

Take the following string containing JSON data:

``` python
json_string = '{"first_name": "Guido", "last_name":"Rossum"}'
```

It can be parsed like this:

``` python
import json
parsed_json = json.loads(json_string)
```

and can now be used as a normal dictionary:

``` python
print(parsed_json['first_name'])
"Guido"
```

You can also convert the following to JSON:

``` python
d = {
    'first_name': 'Guido',
    'second_name': 'Rossum',
    'titles': ['BDFL', 'Developer'],
}

print(json.dumps(d))
'{"first_name": "Guido", "last_name": "Rossum", "titles": ["BDFL", "Developer"]}'
```
