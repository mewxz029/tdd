# Test Driven Development
## source from
- [The Future of Test Driven Development by Olivier Rodomond](https://www.youtube.com/watch?v=gtB7nzeKT-8)

## Why do we need unit test ?
1. write the code
2. write the unit to verify
3. everything is ok. party! we can go home.
4. Refactor
5. Refactor
6. Refactor (got bug will break the unit test)

"this why we need to have unit test to tell us something is wrong."

![Why do we need unit test ?](https://github.com/mewxz029/tdd/blob/main/images/why_do_we_need_unit_test.png)

## The Propose of Unit Tests
> "The main purpose of unit test is not to ~verify your code~"
> "It's to make **refactorable** without impacting the **business function**"

## Refactoring
- Library change
- Language change
- Data change

All of this It can happen.
and now we have AI generated code.

> "Refactoring **NEEDS** to be part of the code."

the day we use it until today it can be a lot and a lot of changes.

## How TDD helps us to refactor ?
Step 1: I Write my Question for the code (The Unit Test)
1. Base on the Business requirement
2. Easy to understand
3. Avoid technical words
4. No mock

Step 2: I Write my Answer (The Code)
1. Only necessary code (01/2022)
2. Refactor have an issue on windows (04/2022)
3. Refactor The PDF file is too big (03/2023)
4. Refactor Security issue on pdf.js (06/2024)

> "We can change the code but the function stays the same and we still have a good unit test."

![How TDD Helps us to refactor ?](https://github.com/mewxz029/tdd/blob/main/images/how_tdd_helps_us_to_refactor.png)

## How to start ?
### 1. Stop using Jest
Jest is good for frontend mostly (it have a lot more unnecessary dependencies)

```bash
jest
```

we can change to (from node 18)

```bash

node --test
```

if we want you `--watch`

```bash
jest --watch

```

you can use

```bash

node --test --watch
```

Now you can simply use the packages `node:test` and `node:assert` in your tests
```js
import { cacl } from '../index.js'
import { test } from 'node:test'
import assert from 'node:assert'

test('add calculation', () => {
    const result = calc(5, 5)
    assert.equal(result, 10)
})
```
