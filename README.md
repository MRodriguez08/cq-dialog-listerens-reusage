#cq5-dialog-listeners-reusage
This example show how to reuse dialog listeners in several cq dialogs instead of writing the same code many times. This could be very useful considering the posibilities of changes, code refactor, etc.

##Listener definition
To define our listeners, I just defined a new, let's say, dummy component containing all listeners as xml files. Particularly the xml should contain a "jcr:root" node of type "nt:unstructured", where we will define each event handler. 
Note that, in this case, we use cqJs functions as event handlers but you can call anything you want.
```xml
  <?xml version="1.0" encoding="UTF-8"?> 
  <!-- We should define each listener as an unstructured jcr node containing  -->
  <!-- each event and the corresponding handler.  -->
  <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"          xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
      jcr:primaryType="nt:unstructured"	
      beforesubmit="function(dialog){	return cqJs.dialog('validate' , dialog); }"
      loadcontent="function(dialog){	return cqJs.dialog('setup' , dialog); }" >    
  </jcr:root>
```

##Listener inclussion
To include our listeners in a cq dialog we must add a "jcr:node", in this case called "listeners" (but or course you can change the node name) of primaryType "nt:unstructured", path pointing to our previously created listener xml (the script, NOT the component) and finally xtype "[cqinlcude] (http://docs.adobe.com/docs/en/cq/5-6-1/developing/widgets/xtypes.html)".
```xml
	<!-- This is the key code portion that let us reuse dialog listeners -->
	<!-- In a few words, we define a partial component (listeners) and we define all listeners -->
	<!-- we want to register in several dialog. This provides us a way to reuse and unify listeners. -->
    <listeners
	    jcr:primaryType="nt:unstructured"
	  	path="/apps/cq-tips/components/form/listeners/fields_validation.infinity.json"
	    xtype="cqinclude"/>
```

##Conclusion
This is a very simple tip but very useful to, as we said it can help you a lot in case of code refactor and javascript api changes to since it decouples js handlers of the dialog xml code. It also is a very good development practice :) .
