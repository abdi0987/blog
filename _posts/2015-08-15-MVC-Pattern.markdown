---
title:  "MVC Pattern"
description: "Introduction to MVC Design Pattern"
## date: add a date when publishing
---

# MVC

MVC is a design pattern that allows you to write more organized and  maintainable code. MVC is used in Frameworks like AngularJS.

## Model 

The model is where our data stored. It does not know about the view or the controller. It notifies its observers about any changes.

## View

View is what's presented to the users and how users interact with the app. The view is made with HTML, CSS, JavaScript and often templates

## Controller

The controller is the glue between the model and view. The contoller is where our business logic well be placed. It removes data , adds data , render ui, etc.

# Example 

![App](../../assets/images/screen2.png "Sample App")

We will be building a simple app that adds and removes li elements. Checkout the github page and follow along with me.

![App](../../assets/images/screen1.png "Sample App")


# Download the starter file from the following [Git Repo](https://github.com/abdi0987/MVC_Example)


## Model 

Don't worry about the index.html file open the app.js file thats what we will focus on.

```javascript
    var model = {
        pizzas: [],
    };
```

This is a very simple model it starts with no data and will add and remove data later on

## View

This is a little more complex

```javascript
    var view = {
        render: function(pizzas) { // Render function runs everytime there is a change
            document.getElementById('collection').innerHTML = '';
            for (var i = 0; i < pizzas.length; i++) {
                //Create the li element
                var node = document.createElement("LI");
                node.className = 'collection-item';

                //Create a element
                var aNode = document.createElement('a');
                aNode.className = 'btn-floating btn-small waves-effect waves-light red right remove-pizza'
                
                //Create icon element
                var iconNode = document.createElement('i');
                iconNode.className = 'material-icons ';

                var iconTextNode = document.createTextNode('clear');
                
                aNode.id = i
                
                iconNode.appendChild(iconTextNode);
                
                aNode.appendChild(iconNode)
                
                node.style.padding= '20px'
                node.style.paddingTop = '10px'

                node.appendChild(aNode)
                

                //Create text node
                var textnode = document.createTextNode(pizzas[i]);
                node.appendChild(textnode);
                

                document.getElementById("collection").appendChild(node);
            }
        },
        
        setEvents : function(){ // add event listener to remove and add pizza buttons
            $(document).delegate('.remove-pizza','click',function(){
                //$(this).parent.style.color = '#e74c3c'
                $(this).parent().css( "color","#e74c3c" )
                controller.removePizza(($(this).parent().context.id))
            })

            $(document).delegate('.add-pizza','click',function(e) {
                controller.addPizza()
                return false
            })
        },
    
    }
```
In the render function we are looping through pizzas and creating a li element for each one 

for the setEvents we are adding event listeners for every button and executing the correct task

## Controller

```javascript
    var controller = {
        init : function() {
            view.render(model.pizzas)
            view.setEvents();
        },
        removePizza: function(id){
            setTimeout(function(){
                model.pizzas.splice(id, 1);
                view.render(model.pizzas);
            }, 500);
        },
        
        addPizza: function(){
            model.pizzas.push("Cheese Pizza")
            view.render(model.pizzas);
        }
    }

```
In the init function we are kicking  off our app. The removePizza function removes the specified element. addPizza function is pushing data to our array the everytime we change the model we call the render function

# Conclusion 

And thats it. If you want to write better code you should look into javascript frameworks like AngularJS. AngularJS was built by a google employee misko. Its maintained by google and it's really popular so you should look into it. :)