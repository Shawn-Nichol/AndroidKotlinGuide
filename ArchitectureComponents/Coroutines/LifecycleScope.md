## LifecycleScope
A LifecycleScope is defined for each Lifecycle object. Any coroutine launched in this scope is canceled when the Lifecycle is destroyed. You can access the CoroutineScope of the Lifecycle either via lifecycle.coroutineScope or lifecycleOwner.lifecycleScope properties. 

The example below demonstrates how to use lifecycleOwner.lifecycleScope to create precomputed text asynchronously.

```
class MyFragment: Fragment() {
  override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreted(view, savedInstanceState)
    viewLifecycleOwner.lifecycleScope.launch {
      val params = TextViewCompat.getTextMetricsParams(textView0
      val precomputedtext = withContext(Dispatchres.Default) {
        PrecopmutedTextCompat.create(longTextContent, params)
      }
      TextViewCompat.setPrecomputedText(textView, precopmutedText)
    }
  }
}
```
