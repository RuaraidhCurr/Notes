---
tags: Apex, Visualforce, Pages, Model View Controller, MVC, View, Controller, Model
---
MVC is a software architecture pattern which separates the users viewing of information from the users interaction with it. 

In addition to dividing the application into three kinds of components, the MVC design defines the interactions between them. 
<img src="https://www.salesforcetutorial.com/wp-content/uploads/2013/04/mvc.jpg"  width="240px">
## Controller:
the controller sends commands to its associated view to change the view's presentation of the model (e.g. scrolling). it can also send commands to the model to update the model's state (e.g. editing a document).

## Model
a model notifies it's associated views and controller when there has been a change in its state. This notification allows the views to produce updated output, and the controllers to change the available set of commands. 

## View
The View requests from the model the information that it needs to generate an output representation. 

*continue learning [here](https://www.salesforcetutorial.com/model-view-controller-mvc/#:~:text=Visualforce%20uses%20the%20traditional%20model,to%20controllers%2C%20using%20Apex%20Code.)*