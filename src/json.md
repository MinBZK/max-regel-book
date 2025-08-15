# Appendix: What is JSON?

JSON (JavaScript Object Notation) is a way to store and share data in a text format that is easy for humans to read and easy for computers to understand. It looks like a list of key-value pairs (like labels and their information).
How to Read It
- **Curly Braces** `{ }`
  
  These wrap an object (a group of related information).

- **Square Brackets** `[ ]`
    
  These wrap a list of items (like a collection of similar things).

- **Keys and Values**

  Each piece of data is a pair:

    "key": "value"
  
  Key = label (always in quotes)

  Value = actual information (can be text, number, true/false, another object, or a list)

Example:

```json
{
    "name": "Alice",
    "age": 30,
    "hobbies": ["reading", "cycling"],
    "address": {
        "city": "Paris",
        "country": "France"
    }
}
```

Reading this out loud:
- name → "Alice"
- age → 30
- hobbies → a list of 2 things: "reading", "cycling"
- address → another object with city and country

> [!TIP]
> Think of JSON like a nested list of labels and details.
> - Objects = groups of info → `{ }`
> - Lists = multiple items → `[ ]`

Even though comments are officially not a part of JSON, it is a common way to insert remarks for human readers: `// comments start with a double slash and is followed by any text until the end of the line`. The text is completely ignored though and has no influence on the values.

So this:

```json
{
  "name": "Alice"
}
```
is equivalent to this:


```json
{ // start of the object
  "name": "Alice" // this person's initial becomes "A"
}
```