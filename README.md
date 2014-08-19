SimpleCallbackManager
=====================

###Simple Asynchronous Callback Manager (C#)


####Instantiate : Create an instance with the final complete callback

	//NOTE: instantiating is also considered an asynchronous callback. Must call cm.done(); after registering all other callbacks.
	//The first argument is "completeCallback" function. It is executed once all registered callbacks are executed. The first parameter passed to this function is the error parameter passed to cm.done(err) function.
	//The last argument is optional "continueOnError" flag. Set this true if you want all registered callbacks to be executed before calling the complete callback even if error occurs. Note that the error parameter passed to the complete callback is going to be the last error occured.

	CallbackManager cm = new CallbackManager(new delegate(Exception err) {
		if(err != null)
			throw err;
		
		Console.WriteLine("all done!");
	});	


####Register - Simple
  //assume there's a SetTimeout async method that takes a System.Action as a first argument and and an integer as a second argument
	SetTimeout(cm.Register(), 100);

####Register - Complex : Wait for a callback then notify the manager that it is done

	cm.wait();
	SetTimeout(delegate() {
		//do another async work
		SetTimeout(delegate() {
			cm.Done();
		}, 100);
	}, 100);

####Handle error : Pass the error from an async callback to the complete callback
	cm.wait();
	SetTimeout(function() {
		cm.Done(throw new Exception("uh oh!"));
	}, 200);


####Finalize : Notify manager that the async callback registeration is done

	cm.Done();
