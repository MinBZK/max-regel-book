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

    {
        "name": "Alice",
        "age": 30,
        "hobbies": ["reading", "cycling"],
        "address": {
            "city": "Paris",
            "country": "France"
        }
    }

name → "Alice"

age → 30

hobbies → a list of 2 things: "reading", "cycling"

address → another object with city and country



> [!TIP]
> Think of JSON like a nested list of labels and details.
> - Objects = groups of info → `{ }`
> - Lists = multiple items → `[ ]`