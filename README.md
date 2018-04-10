# redux-guide
Redux guide
## Redux Help

### Store
```javascript
import rootReducer from './reducers/rootReducer';
import logger from 'redux-logger';
import thunk from 'redux-thunk';

const middleware = [logger, thunk];

const store = createStore(
  // Reducers
  rootReducer,
  // Initial state
  {},
  composeWithDevTools(applyMiddleware(...middleware)),
);
```

### Actions

```javascript
export const TOGGLE_MESSAGE = 'TOGGLE_MESSAGE';

export function toggleMessage() {
    return {
        type: 'TOGGLE_MESSAGE',
    };
}
```

### Reducers
```javascript
import { TOGGLE_MESSAGE } from '../actions/message';

const initialState = {
    messageVisibility: false,
};

export default function (state = initialState, action) {
    const { type, data } = action;
    switch (type) {
        case TOGGLE_MESSAGE:
            return {
                ...state,
                messageVisibility: !state.messageVisibility,
            };
        default:
            return state;
    }
}
```


### rootReducer
```javascript
import { combineReducers } from 'redux';
import message from './message';
import movies from './movies';

const rootReducer = combineReducers({
    message,
    movies
});

export default rootReducer;
```


### Component

```javascript
import React, { Component } from 'react';
import { connect } from 'react-redux';
import { bindActionCreators } from 'redux';

import { toggleMessage } from '../actions/message';

const Toggle = ({ messageVisibility, toggleMessage }) => (
    <div>
        {messageVisibility && <p>You will be seeing this if Redux item is toggled</p>}
        <button onClick={toggleMessage}>
            Toggle Me
        </button>
    </div>
)

const mapStateToProps = (state) => ({
    messageVisibility: state.message.messageVisibility
})

const mapDispatchToProps = (dispatch) => bindActionCreators({
    toggleMessage,
}, dispatch);

export default connect(mapStateToProps, mapDispatchToProps)(Toggle);
```

