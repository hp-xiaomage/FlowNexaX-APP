# Advanced JSON Extraction Rules for Request Response Values and Variable Control

This article explains how to extract content more flexibly from JSON returned by the other party's API in the **Send Request Step**.

The same advanced usage is also supported by **Extract a Value from JSON and Assign It to a New Variable** in variable control.

Telegram contact/link:

https://t.me/flownexax

## Example JSON Data

The following JSON data is used to demonstrate how to extract content:

```json
{
    "store": {
        "book": [
            {
                "category": "reference",
                "author": "Nigel Rees",
                "title": "Sayings of the Century",
                "price": 8.95
            },
            {
                "category": "fiction",
                "author": "Evelyn Waugh",
                "title": "Sword of Honour",
                "price": 12.99
            },
            {
                "category": "fiction",
                "author": "Herman Melville",
                "title": "Moby Dick",
                "isbn": "0-553-21311-3",
                "price": 8.99
            },
            {
                "category": "fiction",
                "author": "J. R. R. Tolkien",
                "title": "The Lord of the Rings",
                "isbn": "0-395-19395-8",
                "price": 22.99
            }
        ],
        "bicycle": {
            "color": "red",
            "price": 19.95
        }
    },
    "expensive": 10
}
```

## Extraction Rule Commands

| Rule Command | Result |
| --- | --- |
| `$.store.book[*].author` | Authors of all books |
| `$..author` | All authors |
| `$.store.*` | Everything under store, including books and bicycle |
| `$.store..price` | Prices of everything under store |
| `$..book[2]` | The third book |
| `$..book[-2]` | The second-to-last book |
| `$..book[0,1]` | The first two books |
| `$..book[:2]` | All books from index 0, inclusive, to index 2, exclusive |
| `$..book[1:2]` | All books from index 1, inclusive, to index 2, exclusive |
| `$..book[-2:]` | The last two books |
| `$..book[2:]` | All books from index 2, inclusive, to the last book |
| `$..book[?(@.isbn)]` | All books with an ISBN number |
| `$.store.book[?(@.price < 10)]` | All books in the store with a price lower than 10 |
| `$..book[?(@.price <= $['expensive'])]` | All books in the store that are not "expensive" |
| `$..book[?(@.author =~ /.*REES/i)]` | All books matching the regular expression, ignoring case |
| `$..*` | Give me everything |
| `$..book.length()` | Number of books |

## Basic Knowledge

JsonPath expressions can use dot notation:

```text
$.store.book[0].title
```

Or bracket notation:

```text
$['store']['book'][0]['title']
```

## Operators

| Operator | Description |
| --- | --- |
| `$` | The root element to query. All path expressions start from this element. |
| `@` | The current node being processed by a filter predicate. |
| `*` | Wildcard. It can be used anywhere a name or number is required. |
| `..` | Deep scan. It can be used anywhere a name is required. |
| `.<name>` | Dot-notated child element. |
| `['<name>' (, '<name>')]` | Bracket-notated child element. |
| `[<number> (, <number>)]` | Array index. |
| `[start:end]` | Array slice operator. |
| `[?(<expression>)]` | Filter expression. The expression must evaluate to a Boolean value. |

## Functions

Functions can be called at the end of a path. The input to the function is the output of the path expression. The function output depends on the function itself.

| Function | Description | Output Type |
| --- | --- | --- |
| `min()` | Provides the minimum value of a numeric array. | Double |
| `max()` | Provides the maximum value of a numeric array. | Double |
| `avg()` | Provides the average value of a numeric array. | Double |
| `stddev()` | Provides the standard deviation value of a numeric array. | Double |
| `length()` | Provides the length of an array. | Integer |
| `sum()` | Provides the sum of a numeric array. | Double |
| `keys()` | Provides property keys, an alternative to the terminal tilde `~`. | `Set<E>` |
| `concat(X)` | Provides a concatenated version of the path output and a new item. | Same as input |
| `append(X)` | Adds an item to the JSON path output array. | Same as input |
| `first()` | Provides the first element of an array. | Depends on the array |
| `last()` | Provides the last element of an array. | Depends on the array |
| `index(X)` | Returns the element at index X in the array. If X is negative, it counts from the end. | Depends on the array |

## Filter Operators

Filters are logical expressions used to filter arrays.

A typical filter is:

```text
[?(@.age > 18)]
```

Here, `@` represents the item currently being processed.

More complex filters can be created with logical operators `&&` and `||`.

String literals must be enclosed in single quotes or double quotes:

```text
[?(@.color == 'blue')]
[?(@.color == "blue")]
```

| Operator | Description |
| --- | --- |
| `==` | Left equals right. Note that `1` is not equal to `'1'`. |
| `!=` | Left is not equal to right. |
| `<` | Left is less than right. |
| `<=` | Left is less than or equal to right. |
| `>` | Left is greater than right. |
| `>=` | Left is greater than or equal to right. |
| `=~` | Left matches the regular expression, for example `[?(@.name =~ /foo.*?/i)]`. |
| `in` | Left exists in right, for example `[?(@.size in ['S', 'M'])]`. |
| `nin` | Left does not exist in right. |
| `subsetof` | Left is a subset of right, for example `[?(@.sizes subsetof ['S', 'M', 'L'])]`. |
| `anyof` | Left intersects with right, for example `[?(@.sizes anyof ['M', 'L'])]`. |
| `noneof` | Left has no intersection with right, for example `[?(@.sizes noneof ['M', 'L'])]`. |
| `size` | The size of the left side, an array or string, should match the size on the right side. |
| `empty` | The left side, an array or string, should be empty. |

## Usage Scenario

This advanced JsonPath rule extraction usage applies to:

- Extracting JSON data from the return value of the FlowNexaX **Send Request Step**.
- Extracting a specific JSON value into a new variable in **Variable Control**.

Use these rules when a simple fixed JSON path is not flexible enough, such as when you need to extract multiple items, filter arrays, match regular expressions, or calculate array length.