Ahh, finally, a short chapter! We'll cover this topic more in the corresponding Lab and just tackle the basic concepts here. 

As stated in the paradigm intro, Event-driven programming revolves around the idea of reacting to events. We define handlers for specific events, and start our own tasks which may themselves wait on events. Sometimes, when running such a task, we need to specify what will happen when a certain event occurs. In this case, we specify a *callback*, which is *the thing we will call when that event occurs*.

Events cannot just be "magically" propagated in your software. At some level, code has to be running that checks whether something has happened and then notifies the things which are interested in that event. This basic code is often called the *event loop* -- this is a segment of code which is constantly running and which collects events that have occurred since the last loop. 

A *handler* is a function which reacts to an event. Usually, the event comes in the form of some object which represents contextual information about the actual event that triggered it. It might look like:
```cpp
	void dragAndDropEventHandler(DragAndDropEvent event) {
		if (event.droppedItem.isFile())
		auto fileData = event.droppedItem.fileData;
		std::cout << "Loading file: " << fileData.filename << std::endl;
		myViewerService->loadFile(fileData.body);
		// and loadFile might itself fire off some other event (LoadedFileEvent?) ...
	}
```

Sometimes, an object intrinsically knows that it should handle events in some way, and our definition is just implementing the "how" (usually by overriding the otherwise-empty handler). Other times, we need to explicitly mark which objects are interested in which events. This is sometimes called "publish-subscribe" or "the observer pattern".

In the lab segment, we will use the **Qt Framework** to create simple GUI apps which can respond to events.
Qt introduces a new set of keywords to C++, which allow you to establish "signals" and "slots". You can hook up two objects with the "connect" function built into qt, connecting a type of signal from one object to a slot on another object. Then, any time that object "emits" the signal, the second object will be notified by the slot being called. The Qt framework helps fill in all necessary code to make this happen.

Worth noting is that sometimes we don't have source-level control over the classes that handle this. Sometimes we need to provide a handler function in the form of a *callback*. When we provide a *callback* to an object or function, we are saying "this is the function I want you to call when the thing I am interested in happens". This is a little bit of a lower level approach. For example, there may be a function `startChargingEnergy`, which takes a callback for what to do when energy is sufficiently charged:

``` c
// Syntax for this in C is really dumb...
void startChargingEnergy(int threshold, void (*callback)()) {
	int energy = 0;
	int chargeRate = 10;
	while (energy < threshold) {
		std::cout << "Charging... "  << energy << "!" << std::endl;
		energy += chargeRate;
	} 
	// When done...
	callback();
}
void Kamehameha() { createBigRay(); }
startChargingEnergy(100, Kamehameha);
```
``` cpp
// Syntax for this in C++ can be many things, but this is reasonable
void startChargingEnergy(int threshold, std::function<void()> callback) {
	int energy = 0;
	int chargeRate = 10;
	while (energy < threshold) {
		std::cout << "Charging... "  << energy << "!" << std::endl;
		energy += chargeRate;
	} 
	// When done...
	callback();
}

void Kamehameha() { createBigRay(); }
startChargingEnergy(100, Kamehameha);
```


Note that here we are **not** passing the result of the function, we are passing *the function itself.* Remember, the function name is a *reference* to the function. We only get a result when we call that function with the () operator.

Of course, nothing stops us from passing in any number of functions. We might have one for when charging succeeds, one for when it fails, one for when it is interrupted... be creative.
This sort of pattern is used often in the JavaScript world, where we are often invoking queries to some other web element which might (or might not) succeed. In these cases, we generally provide a success handler and an error handler.

## Event Loop Considerations

It is important to note that while an event loop mentally suggests something running *in the background*, perhaps even *in parallel* with our actual event code, these are **NOT** a requirement for event driven programming, and you should not assume that any event system allows them. The event loop can be fully synchronous code that handles events in linear order, and which locks up while each event handler is running.

Right now, all code we have written is *synchronous*, i.e. it runs in a specific order. Furthermore, all of our code can run *single-threaded*, i.e. on a single core machine. This is in comparison to *asynchronous* code, which executes in a potentially unknown order (and which also usually uses a loop) and *parallel* code, where multiple code segments can be executing at the same time on different threads or processes.

We have not explored these topics yet, but I want to clear this up now so that you are not lead astray. Do not confuse these concepts with the event loop concept. The only thing the event loop does is change when we run functions, it does not make our code parallel or asynchronous or anything like that. 

To drive this point home, I suggest that you try running the code above inside a loop of some kind. You will find that every time the outer loop iterates, the *full charging loop finishes* and calls Kamehameha, and *only then* does the outer loop go to the next iteration. There is no free lunch.


