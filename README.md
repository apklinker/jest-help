# Jest Reference

Basic building blocks
```jsx
describe('some set of tests/its', _ => {
  it('should be something', _ => {
    expect(true).toBe(true);
  });
});
```

Test blocks
```jsx
test('some action', _ => {
  expect(true).toBe(true);
});
```

# Specific Tests

## Component Boilerplate
```jsx
import ComponentName from './ComponentName';

const componentName = renderer.create(<App />);    // Not used in tests, just generates next objects
const json = componentName.toJSON();               // Sometimes used, confirm data/layoutn orders
const root = componentName.root;                   // findByProps(...), findByType(OtherComponentName), find...
const instance = root.instance;                    // props, state, member methods/variables

describe('ComponentName', _ => {
    ...
});
```

## Mocking...

### Mocking Fucntions
```jsx
const myMock = jest.fn();
console.log(myMock());
// > undefined

myMock
  .mockReturnValueOnce(10)
  .mockReturnValueOnce('x')
  .mockReturnValue(true);

console.log(myMock(), myMock(), myMock(), myMock());
// > 10, 'x', true, true
```

### Mocking Modules
```jsx
// users.js //////////////////////////////////////////////////
import axios from 'axios';

class Users {
  static all() {
    return axios.get('/users.json').then(resp => resp.data);
  }
}

export default Users;


// users.test.js /////////////////////////////////////////////
import axios from 'axios';
import Users from './users';

jest.mock('axios');

test('should fetch users', () => {
  const resp = {data: [{name: 'Bob'}]};
  axios.get.mockResolvedValue(resp);
  // or you could use the following depending on your use case:
  // axios.get.mockImplementation(() => Promise.resolve(resp))
  return Users.all().then(users => expect(users).toEqual(resp.data));
});
```

## Spying...

### Spying On Class Methods
```jsx
class TestComponent extends Component {
  ...
  test(x) {
    if (x) a();
    else b();
  }
  a() {}
  b() {}
  ...
}

// Testing a and b being called
describe('Ensure test() calls a() and b() propperly', _ => {
  const testComponent = render.create(<TestComponent />).instance;
  const callsA = [ 1, 'test', new Date() ];
  const callsB = [ 0, undefined, null ];
  var aSpy = jest.spyOn(TestComponent, 'a');
  var bSpy = jest.spyOn(TestComponent, 'b');
  
  beforeEach(_ => {
    aSpy.mockClear();
    bSpy.mockClear();
  });
  
  callsA.forEach(item => {
    it('calls a() correctly for ' + item, _ => {
      testComponent.test(item);
      expects(spyA).toBeCalled();
    });
  });
  callsB.forEach(item => {
    it('calls b() correctly for ' + item, _ => {
      testComponent.test(item);
      expects(spyB).toBeCalled();
    });
  });
});
```

### Spying On Module Methods
----- TODO: Don't know how yet -----

## Redux

### Actions
```jsx
// _____.js
function addTodo(content) {
    return {
        type: ADD_TODO,
        data: content,
    };
}

// _____.test.js
expect(addTodo('Todo 1')).toEqual({
    type: ADD_TODO,
    data: 'Todo 1',
});
```

### Reducers
```jsx
// _____.js //////////////////////////////////////////////////
function todoReducer(state, action) {
    if (action.type === ADD_TODO) return Object.assign({}, state, {
        todos: [
            ...state.todos,
            todo
        ]
    });
    else ...
}

// _____.test.js /////////////////////////////////////////////
const mockState = {
    todos: [],
};
const resultingState = addTodo(mockState, 'testTodo');
expect(resultingState.todos).toContain('testTodo');
expect(resultingState.todos.length - mockState.todos.length).toBe(1);
```

### Sagas
----- TODO: Tanner maybe? -----
