# Shared ViewModel
Is a ViewModel that shares data across multiple components this allows for easier component communication. 

```
class MySharedViewModel : ViewModel() {
  val _counter: MutableLiveData<Int> = MutableLiveData()
  val couner: LiveData<Int> = _counter
  
  fun counterUp() {
    _counter.value = (_counter.value ?: 0) + 1
  }
  
  fun counterDown() {
    _counter.value = (_counter.value ?: 0) - 1
  }  
}


class FragmentOne : Fragment() {
  private lateinit var binding: FragmentOneBinding
  private viewModel: MySharedViewModel
  
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
        
    val viewModelFactory: OneFragmentViewModelFactory = OneFragmentViewModelFactory(8)
    viewModel = ViewModelProvider(requireActivity(),viewModelFactory).get(MySharedViewModel::class.java)
  }
  
  override fun onCreateView(
    inflater: LayoutInflater,
    container: ViewGroup?,
    savedInstanceState: Bundle?
  ): View? {
  
    // arguments the inflater from the activity, layout you are using, ViewGroup of the activity, don't attach to parent.
    binding = DataBindingUtil.inflate(inflater, R.layout.fragment_one, container, false)

    binding.binding = this

    // Specify the fragment view as the Lifecycle owner of the binding, This is done so the that binding
    // can observer LiveData updates.
    binding.lifecycleOwner = this

    // THis allows the bound layout to access all the data in the ViewModel
    binding.viewModel = viewModel

    return binding.root
  }
}


class TwoFragment : Fragment() {
  private lateinit var binding: FragmentTwoBinding
  val viewModel: MySharedViewModel by activityViewModels()

  override fun onCreateView(
    inflater: LayoutInflater, container: ViewGroup?,
    savedInstanceState: Bundle?
    ): View? {

      binding = DataBindingUtil.inflate(inflater, R.layout.fragment_two, container, false)

      binding.binding = this
      binding.lifecycleOwner = this

      // This allows the bound layout to access all the data in the ViewModel
      binding.viewModel = viewModel

      // Inflate the layout for this fragment
      return binding.root
  }
}
```

Notice that both fragmetns retrieve the activity that contains them. That way, when the fragments each get the ViewModelProvider, they receive the same Shared ViewModel instance, which is scoped to this activity. In order to use  "by activityViewModel()" you will need to implement the following depency in the build.gradle 

```
    def fragment_version = "1.2.5"
    // Kotlin
    implementation "androidx.fragment:fragment-ktx:$fragment_version"
```

This approach offers the following
- Activith does not need to do anything, or know aything about this communication
- Fragments don't need to do anything, or know anything about this communication. 
- Fragment has its own lifecycle and is not affected by the lifecycle of the other one. If one fragment rplaces the other one, the UI continues to work without any problem. 
