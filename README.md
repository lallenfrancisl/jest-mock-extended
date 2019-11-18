# jest-mock-extended
Type safe mocking extensions for Jest

# Installation
```
npm install jest-mock-extended --save-dev
```
or
```
yarn add jest-mock-extended --dev
```

# Example

```js
import mock from 'jest-mock-extended';

interface PartyProvider {
   getPartyType: () => string;
   getSongs: (type: string) => string[]
   start: (type: string) => void;
}

describe('Party Tests', () => {
   test('Mock out an interface', () => {
       const mock = mock<PartyProvider>();
       mock.start('disco party');
       
       expect(mock.start).toHaveBeenCalledWith('disco party');
   });
   
   
   test('mock out a return type', () => {
       const mock = mock<FunProvider>();
       mock.getPartyType.mockReturnValue('west coast party');
       
       expect(mock.getPartyType()).toBe('west coast party');
   });
});
```

# calledWith() Extension

```jest-mock-extended``` allows for invocation matching expectations. Types of arguments, even when using matchers are type checked.

```
const provider = mock<PartyProvider>();
provider.getSongs.calledWith('disco party').mockReturnValue(['Dance the night away', 'Stayin Alive']);
expect(provider.getSongs('disco party')).toEqual(['Dance the night away', 'Stayin Alive']);

// Matchers
provider.getSongs.calledWith(any()).mockReturnValue(['Saw her standing there']);
provider.getSongs.calledWith(anyString()).mockReturnValue(['Saw her standing there']);

```
You can also use calledWith() on it's own to create a jest.fn() with the calledWith extension:

```
 const fn = calledWithFn();
 fn.calledWith(1, 2).mockReturnValue(3);
```

## Available Matchers

### any..
any(), anyString(), anyNumber(), anyFunction(), anySymbol(), anyObject(), anyArray(), anyMap(), anySet()


| Matcher               | Description                                                           |
|-----------------------|-----------------------------------------------------------------------|
|eq(any)                | e.g eq(1), required when using other matchers in a list of arguments. |
|isA(class)             | e.g isA(DiscoPartyProvider)                                           |
|includes('value')      | Checks if value is in the argument array                              |
|containsKey('key')     |  Checks if the key exists in the object                               |
|containsValue('value') | Checks if the value exists in an object                               |
|has('value')           | checks if the value exists in a Set                                   |
|notNull()              | value !== null                                                        |
|notUndefined()         | value !== undefined                                                   |
|notEmpty()             | value !== undefined && value !== null && value !== ''                 |

