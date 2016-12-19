You can navigate between the methods in your bindings and the associated steps in your Gherkin feature files. 

#Navigating from a Binding to Steps in Gherkin Files
To navigate from a step definition method to the matching step(s) in your Gherkin feature file(s):  

1. Place your cursor in a step definition method. 
1. Right-click and select "Go To SpecFlow Step Definition Usages" from the context menu, or simply press Alt+Ctrl+Shift+S (configurable). 
1. If only one match exists, the feature file is opened. If more than one matching step is defined in your feature files **NOT TESTED**, select the corresponding feature file from the list to switch to it.

#Navigating from a Gherkin Step to a Binding
To navigate from a step in a Gherkin feature file to the corresponding step definition method: 

1. Place your curosr in the step in your feature file.
1. Right-click and select "Go To Step Definition" from the context menu, or use the Alt+Ctrl+Shift+S.
1. The file containing the binding is opened at the appropriate step definition method.  
  **Note:** If the steps definition does not exists, a message is displayed instead:  
  [![Not existing steps definition](http://i734.photobucket.com/albums/ww347/rommelmanalo/Specflow/NotExistingDefinition.png)](http://i734.photobucket.com/albums/ww347/rommelmanalo/Specflow/NotExistingDefinition.png)
_Figure 5: Not existing steps definition (click image for full size)_  
  Click on Yes to copy the skeleton code for your step to the clipboard, so you can paste it in the corresponding code file.