## Motivation

This is fork from [sequelize-test-helpers](https://github.com/davesag/sequelize-test-helpers). This library cut down features from original library and make it testing framework agnostick.

## Getting started

A library for unit testing your [Sequelize](http://docs.sequelizejs.com) models and code that needs those models.

[![NPM](https://nodei.co/npm/sequelize-easy-mock.png)](https://nodei.co/npm/sequelize-easy-mock/)

### Installation

Add `sequelize-easy-mock` as a `devDependency`:

```sh
npm i -D sequelize-easy-mock
```

## Examples

### Unit testing models created with `sequelize.define`

**Note**: See below for how to test models created using `Model.init`

Let's say you have a Sequelize model `User` as follows:

#### `src/models/User.js`

```js
const model = (sequelize, DataTypes) => {
  const User = sequelize.define(
    'User',
    {
      age: {
        type: DataTypes.INTEGER.UNSIGNED
      },
      firstname: {
        type: DataTypes.STRING,
        allowNull: false,
        validate: {
          notEmpty: true
        }
      },
      lastname: {
        type: DataTypes.STRING,
        allowNull: false,
        validate: {
          notEmpty: true
        }
      },
      email: {
        type: DataTypes.STRING,
        allowNull: false,
        unique: true,
        lowercase: true,
        validate: {
          isEmail: true,
          notEmpty: true
        }
      },
      token: {
        type: DataTypes.STRING,
        validate: {
          notEmpty: true
        }
      }
    },
    {
      indexes: [
        { unique: true, fields: ['email'] },
        { unique: true, fields: ['token'] }
      ]
    }
  )

  User.associate = ({ Company }) => {
    User.belongsTo(Company)
  }

  return User
}

module.exports = model
```

#### `test/unit/models/User.spec.js`

```js
const { sequelize, dataTypes } = require('sequelize-easy-mock')

const UserModel = require('../../src/models/User')

describe('src/models/User', () => {
  const User = UserModel(sequelize, dataTypes)
  const user = new User()

  // TODO: Write your teste here
  // ...
})
```

### Prerequisites

- [NodeJS](htps://nodejs.org), version 8.10.0 or better (I use [`nvm`](https://github.com/creationix/nvm) to manage Node versions — `brew install nvm`.)

### Initialisation

```sh
npm install
```

### Test it

- `npm test` — runs the unit tests
- `npm run test:unit:cov` — runs the unit tests with code coverage
- `npm run test:mutants` — runs the mutation tests

### Lint it

```sh
npm run lint
```
