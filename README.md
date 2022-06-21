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