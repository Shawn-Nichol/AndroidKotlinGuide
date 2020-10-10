# How to sort liveDataLists
1) Convert LiveData to MutableList<User>
```
// Conver LiveData to MutableList<User>
val userList: MutableList<User>? = viewModel.userList?.value as MutableList<User>?

// Create a new list so DiffUtil can compare the difference. 
val orderList = userList?.toMutableList()

// Sort list
orderList?.sortBy { it.accountNumber }

// post new list to LiveData so DiffUtil can compare the difference
viewModel.userList?.postValue(orderList)

```

