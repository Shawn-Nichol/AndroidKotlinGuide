# Switch
A switch is a two-state toggle switch widget that can select between two options. The user may drag the thumb backc and forht to choose thee selected option or simply tap to toggle as if it were a checkbox. The text property controls the text displayed in the label for the switch, whereas the off and on text ctonrols the text on the thumbSimilarly, the textAppearance and the related setTypeFace() ethods control the typeface and styleo f label text, whereasas the switchTextAppearance and the related setSwitchTypeface() methods control that of the thumb. 

## Data binding implementation
xml
onChecked="@= ..." the equal sign is for twoway data binding.  
```
        <androidx.appcompat.widget.SwitchCompat
            android:id="@+id/switch_one_frag1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:checked="@={binding.switchOneState}"
            android:onCheckedChanged="@{(button, bool) -> binding.switchOne(bool)}"
            />
```

fragment
```
    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        binding = DataBindingUtil.inflate(inflater, R.layout.fragment_one, container, false)
        binding.binding = this
        binding.lifecycleOwner = this

        return binding.root
    }
    
        fun switchOne(state: Boolean) {
        Log.i(TAG, "switchOne boolean $state")
    }
```
