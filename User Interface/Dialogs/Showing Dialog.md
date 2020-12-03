## Showing Dialog

With fragment Transaction
```
fun launchDialog() {
  val dialog  = MyAlertDialog()
  dialog.show()
}
```


Navigation component

```
fun launchDialog() {
  findNavController().navigate(MyFragmentDirections.actionMyFragmentToMyAlertDialog())
}
```
