LiveDAta subclass which may observe other LiveData objects and react on OnChanged events from them. Use Mediator live data to combine two LiveData lists. 

This class correctly propagtess its active/inactive states down to source LiveData objects. 

Consider the following scenario: we have 2 instances of LiveData, let's name the liveData1 and liveData2, and we want ot merge their emissions in one object: liveDataMerger. Then liveData1 and liveData2 will become sources for the MediatorLiveData liveDataMerger and every time onCahgned callback is called for either of them we set a new value in liveDataMerger. 

```
LiveData<integer>
  liveData1 = ...;
  LiveData
 <integer>
   liveData2 = ...; MediatorLiveData
  <integer>
    liveDataMerger = new MediatorLiveData&lt;&gt;(); liveDataMerger.addSource(liveData1, value -&gt; liveDataMerger.setValue(value)); liveDataMerger.addSource(liveData2, value -&gt; liveDataMerger.setValue(value)); 
  </integer>
 </integer>
</integer>
```

Let's consider that we only want 10 values emitted by liveData1, to be merged in the liveDataMerger. Then after 10 values, we can stop listening to liveData1 and remove it as a source. 

```
liveDataMerger.addSource(liveData1, new Observer<integer>
 () {
       private int count = 1;
 
       @Override public void onChanged(@Nullable Integer s) {
           count++;
           liveDataMerger.setValue(s);
           if (count &gt; 10) {
               liveDataMerger.removeSource(liveData1);
           }
       }
  });
  
</integer> }
  }
```
