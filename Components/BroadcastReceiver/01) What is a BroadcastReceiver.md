# BroadcastRecevier
A Broadcast recevier allows apps to send or receive broadcast messages from the Android system and other apps, similar to the publish-subscribe design pattern. These boradcasta are sent when an event of interest occurs. For example, the Android System sendsd broadcast when various system events occur, such as when the system boots up or the device starts charging. Apps can also send custom broadcasts, to notify other apps of somethign that they might be interested in. 

Apps can register to receive specific broadcast when a broadcast is sent, the sytem automatically routes broadcasts to all apps that have subscribed to receive that particular type of broadcast. The broadcast message is wrapped in an Intent object whose action string identifies the event that occurred. For example the aireplane mode intent includes a boolean extra that  indicates whether or not Airplane mode is on. 

For a complete list of system broadcast actions, see BROADCAST_ACTIONS.TXT in the Android SKD. Each broadacast has a constant filed associated with it. For example, the value of the constant ACTION_AIRPLANE_MODE_CHANGED  is android.intent.action.AIRPLANE_MODE

## Restrictions
Permission all you to restrict  broadcast to the sest of apps that hold certain permissions. You can enforce restrictions on either the sender or the receiver of a broadcast. 

## Security considerations and best practices.
- If you don't need to send a broadcast to components outside of your app, then send and receive local broadcasts with the LocalBroadcastManager wich is available in the Support Library. THe LocalBroadcastManager is much more efficient (no interprocess communication needed) and allows you to avoid thinking about any security issues related to other apps being able to receive or send your broadcasts. Local Broadcasts can be used as a general purpose pub/sub event bus in your app without any over heads of system wide broadcasts. 

- If many apps have registered to receive the same broadcast in their manifest, it can cause the sytem to alucn a lot of apps, causing a substaintial impact on both device performance an user experince. To avoid this, prefer using context registration over manifest declaration. Sometimes, the Android System itself enforces the use of context-registered receivers. For example, the CONNECTIVITY_ACTION broadcast is delivered only to context-registered receivers. 

- Do not broadcast sensitive information using a implicit intent the information can be read by any app that registers to receive the broadcast. There are three ways to control who can receive your broadcast.
  - You can specify a permission when sending a broadcast. 
  - Android 4.0 and higher, can specify a package with setPackage(String) when sending a broadcast. The system restrictis the broadcast to the set  of apps that match the package. 
  - You can limit yourself to only local broadcasts with LocalBroadacstManager. 
  
  The Name space of braodcast actions is global. make sure that actions names and other strings are written in a namespace you own, or else you may inavertenly conflict with other   apps.   

- Becuase a receiver's on Receive(Context, intent) method runs on teh main thread, it should execute and return quickly. If you need to perform long running work, be careful about spawning threads or staarting background services becuase the sytem can kill the entire process after onReceive() returns. 
  - Calling goAsync() in your reciever's onReceive() method and passing teh BroadcastReciever. PendingResult to a background to finsih with the broadcast very qickly. It does allow you to move work to another thread toa void glitching the mainthread. 
  - Scheduling a job with JobSechduler.

- Do not start activites from broadacst receivers becuase the user experince is jarring; especially if there is more than onreceiver. Instead consider displaying a notification. 

