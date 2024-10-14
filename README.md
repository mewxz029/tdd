# Test Driven Development
## source from
- [The Future of Test Driven Development by Olivier Rodomond](https://www.youtube.com/watch?v=gtB7nzeKT-8)
- [Creatorsgarten](https://creatorsgarten.org/videos/bkkjs21/tdd)

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
### Stop using Jest
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
import instance from 'module'
import { test, mock } from 'node:test'
import assert from 'node:assert'

test('add calculation', () => {
    // mock.instance.calculate
    mock.method(instance, 'calculate', () => 10)
    const result = calc(5, 5)
    assert.equal(result, 10)
})
```

### His TDD Setup (VIM + TMUX)
![His TDD Setup (VIM + TMUX)](https://github.com/mewxz029/tdd/blob/main/images/his_tdd_setup.png)

### How to Mock API in 2024?
Stop using axios and start using [Undici](https://github.com/nodejs/undici)
```js
import axios from 'axios';
```
Undici is the top 1 HTTP client built by Core NodeJs Team
```js
import { request } from 'undici';
```

Let's have a small example to get data from Github
```js
import { request } from 'undici'

async function retrieveGithubProfile(name) {
    const url = 'https://api.github.com/' + name
    const { body } = await request(url)
    return body.json()
}
```

We don't mock the Library we ***intercept*** the API request
```js
import { MockAgent, setGlobalDispatcher } from 'undici'
import { test } from 'node:test'
import { retrieveGithubProfile } from '../index.js'
import assert from 'node:assert'

const mockAgent = new MockAgent()
setGlobalDispatcher(mockAgent)

test('Retrieve github info', async () => {
    mockAgent
     .get('https://api.github.com')
     .intercept({ method: 'GET', path: '/mewxz029' })
     .reply(200, { foo: 'bar' })
    const result = await retrieveGithubProfile('mewxz029')
    assert.deepEqual(result, { foo: 'bar' })
})
```
The Undici interceptor can catch any request made by majorities of the HTTP client(axios, got, etc...)


### How to Mock MongoDB in 2024?
Stop ~mocking~ mongo, just run it in ***docker***
```js
import { MongoDBContainer } from '@testcontainers/mongodb'
```
[Testcontainers](https://testcontainers.com/) is using docker to start and kill a container just for testing purposes

![Testcontainers Workflow](https://github.com/mewxz029/tdd/blob/main/images/testcontainers_test_workflow.png)

### How to use TestContainer?
Let's get our question
> To prepare the test we insert fake data in the db
```js
import { MongoDBContainer } from '@testcontainers/mongodb'
import { getDB, getCars } from '../index.js'

let container, db, config

before(async () => {
    container = await new MongoDBContainer().start()
    config = {
        host: container.getHost(),
        port: container.getMappedPort(27017)
    }
    db = (await getDB(config))
})

test('Search for car info', async (e) => {
    await db.collection('cars1').insertMany({ foo: 'bar1' })
    const result = await getCars('cars1')
    assert.equal(result.length, 1)
    assert.deepEqual('bar1', result[0].foo)
})
```

Time to answer
> Let's try to answer with mongodb client or mongoose

**mongodb**
```js
import { MongoClient } from 'mongodb'

// function to connect to MongoDB
export async function getDB(config) {
    // ...
}

// function to retrieve car information from a collection
export async function getCars(collectionName, c) {
    const db = await getDB(c)
    const collection = db.collection(collectionName)
    return await collection.find({}).toArray() // Get all document
}
```
**mongoose**
```js
import mongoose from 'mongoose'

// function to connect to MongoDB using Mongoose
export async function getDB(config) {
    // ...
}

const carSchema = new mongoose.Schema({
    foo: String
})

const Car = mongoose.model('Car', carSchema)

// function to retrieve car information from a collection
export async function getCars(collectionName) {
    return await Car.find({}).exec() // Get all documents the MongoDB collection Car
}
```

### What about AI?
AI can generate code but most of the engineer use it the wrong way!

ChatGPT is the best partner for Test Driven Development!

Since The test is the ***question***, Then the test should be the ***prompt***

### Conclusion
- Unit Test allows safe refactoring
- Mock are the enemy of refactoring
- Intercept instead of mocking
- Use AI like a pro
- Open source is the best way to practice TDD
- test driven development will get you a job easily
