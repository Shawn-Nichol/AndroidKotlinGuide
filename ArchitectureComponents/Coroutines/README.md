## How do Koltin Coroutines work with Architecture componetnes
Kotlin coroutines provide an API that enables you to write asynchronous code. With kotlin coroutines, you can define a CoroutineScope which helps you to manage when your coroutines should run. Each asynchronous operation runs within a particular scope. 

Architecture components proide first-class support for coroutines for logical scope in your app along with an interoperability layer with LiveData. T

## Add Dependencies

The built-in coroutine scope described in this topic are continaed in the KTX extensions for each corresponding Architecture component. 

- For ViewModelScope, use Androidx.lfiecycle:lifecycle_viewModel-ktx:2.1.0

- LifecycleScope: use Androidx.lifecycle:lifecycle-runtime-ktx:2.2.0-alpha01

- LiveData: use androidx.lifecycle:lifecycle-livedata-ktx:2.2.0-alpha01

## Lifecycle-aware coroutine scopes 
Architecture components defines the following built-in scopes that you can use in your app

- ViewModelScope
A ViewModelScope is defined for each ViewModel, Any coroutine launched in this scope is automatically canceled if the ViewModel is cleared.

- LifecycleScope
A LivecycleScope is defined for each Lifecycle object. Any coroutine launched in this scope is canceled when the Lifecycle is destroyed.
- LiveData







