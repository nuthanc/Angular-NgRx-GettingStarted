# Angular-ngrx-GettingStarted
Materials for NgRx course.

`APM-Demo0`: The starter files for this course. **Use this to code along with the course**.

`APM-Demo1`: Completed files after the *First Look at NgRx* module. It demonstrates a very simple NgRx example.

`APM-Demo2`: Completed files after the *Strongly Typing Actions with Action Creators* module. It refactors the simple example to include developer tooling support and strong typing.

`APM-Demo3`: Completed files after the *Working with Effects* module. It adds an effect to retrieve data via http. NOTE: Once we move the data retrieval to actions and the store, the create, update, and delete operations no longer work. These features are implemented with the store in the next demo.

`APM-Demo4`: Completed files after the *Performing Update Operations* module. It adds the code needed for create, update, and delete operations via http.

`APM-Demo5`: Completed files after the *Architectural Considerations* module. It implements the container/presentational component pattern and the OnPush change detection strategy.

NOTE:
- June 30, 2020: This code was modified to Angular version 9 and NgRx version 9. See the CHANGELOG.md file for details.

### Ngrx Store

* Single container for application state
* Interact with that state in an immutable way
* Install the @ngrx/store package
* Organize application state by feature
  * State is not created for Module that is not loaded
  * Feature Module State Composition
```ts
// Sub-slice of state
StoreModule.forFeature('products', {
  productList: listReducer,
  productData: dataReducer
})
```
* Name the feature slice with the feature name
* Initialize the store using:
```ts
StoreModule.forRoot(reducer)
StoreModule.forFeature('feature', featureReducer)
```
* Define an action for each event worth tracking

### Dispatching an Action and Subscribing to the Store

* Often done in response to a user action or an operation
* Inject the store in the constructor
* Call the dispatch method of the store
* Pass in the action to dispatch
* Subscribing to the store often done in the ngOnInit Lifecycle hook

### Strongly Typing State

* Define an interface for each slice of state
* Compose them for the global application state
* Use the interfaces to strongly type the state throught the application
* Initialize initial state in Reducers
* Build selectors to define reusable state queries
  * Conceptually similar to stored procedures
* Define a Feature Selector to return a feature slice of state
```ts
const getProductFeatureState = createFeatureSelector<ProductState>('products');
```
* Use General selector to return any bit of state from the store by composing selector functions to navigate down the state tree
```ts
export const getShowProductCode = createSelector(
  getProductFeatureState,
  state => state.showProductCode
)
```

### Strongly Typing Actions

* Define the appropriate actions
* Export a constant that call createAction
* Specify a clear, unique action type string as the 1st argument
```ts
export const toggleProductCode = createAction('[Product] Toggle Product Code');
```
* Use props to define associated data as needed
```ts
export const setCurrentProduct = createAction(
  '[Product] Set Current Product',
  props<{ product: Product }>()
);
```
* For complex operation, define multiple actions(Additional success and fail actions)
```ts
export const loadProducts = createAction(
  '[Product] Load'
);

export const loadProductsSuccess = createAction(
  '[Product] Load Success',
  props<{ products: Product[] }>()
);

export const loadProductsFailure = createAction(
  '[Product] Load Fail',
  props<{ error: string }>()
);
```

### Working with Effects

* ng add @ngrx/effects
* Build the effect to process that action and dispatch the success and fail actions
* Initialize the effects module in the root module(ng add does this automatically)
* Register effects in your feature modules
* Process the success and failure actions in the reducer

### Performing Update Operations with Side Effects

* Identify the state and actions
* Strongly type the state and build selectors
* Strongly type the actions with action creators
* Dispatch an action to kickoff the operation
* Build the effect to perform the operation and dispatch a success or fail action
* Process the success and fail actions in the reducer