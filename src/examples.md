# Use cases and examples

We'll look at the earlier examples of business rules to determine discount on tickets and implement them with the tools we have under our belt.

The following factset with part `persons`: \\(FS_{persons}\\) is used again.

| name   | hair   | weight | age | gender | member |
|--------|--------|-------:|----:|--------|--------|
| Homer  | short  |    250 |  36 | male   | false  |
| Marge  | long   |    150 |  35 | female | false  |
| Bart   | short  |     90 |  10 | male   | false  |
| Lisa   | middle |     78 |   8 | female | false  |
| Maggie | middle |     20 |   1 | female | false  |
| Abe    | short  |    170 |  70 | male   | false  |
| Selma  | long   |    160 |  41 | female | true   |
| Otto   | long   |    180 |  38 | male   | false  |
| Krusty | middle |    200 |  45 | male   | true   |


> [!WARNING]
> TODO

### Example: Personal Discount Ticket

- **Rule**: If a person is a member and the total is over $100, give 10% off.

The engine checks if both conditions are true, and if so, applies the discount.


```
members = getpart("persons") ; filter (member == true)
discount = returnif( getpart("members"), const({factor: 0.15})) ; const({factor: 0.0})
  
```

### Example: Family Discount Ticket

- **Rule**: If the group consists of more than 2 members, all persons younger than 12 years get 50% off.

The engine checks the group of people, finds the kids if there are any, and and if so, applies the discount.

### Example: Combined Discount

- **Rule**: If there is Family Discount use that, otherwise consider the personal discount. If both don't hold, give 5% discount on week days.