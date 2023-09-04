# DouHaoCMS has a cross site request forgery vulnerability

Version DouhaoCMSV3.3

Vulnerability Introduction:
DouHaoCMS has a CSRF vulnerability, which allows attackers to add backend administrator accounts to gain privileges

Code exists in file
adminAction.class.php

![图片](https://github.com/mnbvcxz131421/douhaocms/assets/102493444/1caa73d6-d735-432e-a230-d2b14c869cdb)

Define an adminActionn class
_ initialize() method is a constructor that executes first when the controller is instantiated. It called parent::_ Initialize() to execute the constructor of the parent class BackendAction, and then create a database model object $this ->_ Mod


 ![图片](https://github.com/mnbvcxz131421/douhaocms/assets/102493444/40c6f698-de59-41d8-82f0-eff50d3b5de4)

Define a_ Before_ Index() class
'title ': Title, displayed as' Add Administrator'.
'iframe': A URL address pointing to admin/add
'id': The ID of the element, set to 'add'.
'width': The width of the pop-up window, set to 500 pixels.
'height': The height of the pop-up window, set to 210 pixels
Add $big_ Menu assigned to template variable 'big'_ Menu '


![图片](https://github.com/mnbvcxz131421/douhaocms/assets/102493444/c9ecab29-fde5-4a48-9960-08413074e44a)

 
Find the BackendAction class and search for add in the BackendAction.class.php file
Create a database model object $mod
Check if the request is a POST request, and if it is a POST request, it indicates that the user is submitting form data.
Create a data object using the $mod ->create() method. If data validation fails, false will be returned.
If present_ Before_ Insert method, which will be called to process data
Use the $mod ->add ($data) method to add data to the database. If the addition is successful, it will execute_ After_ Insert
If the addition is successful, a prompt message indicating successful operation is returned. If the addition fails, a prompt message indicating the operation failed is returned.


 ![图片](https://github.com/mnbvcxz131421/douhaocms/assets/102493444/5121362b-7495-4ee8-a8ad-40eb1905955d)


Define a _ Before_ Add
Use M ('admin role ') ->where ('status=1') ->select(); Query admin in the database_ Role table and filter records with a status field of 1, then copy the results to the variable role_ List

 ![图片](https://github.com/mnbvcxz131421/douhaocms/assets/102493444/1f32a609-d2fa-4f24-b182-e184a16d7629)

Define a _ Before_ Insert
First, determine if the password is empty. If so, delete the field from the $data array. Otherwise, perform MD5 function encryption and return it to the $data array to insert the array into the database
_ Before_ Edit
Call_ Before_ Add to avoid duplicate code


 ![图片](https://github.com/mnbvcxz131421/douhaocms/assets/102493444/4d0b72f0-e9ea-4aa7-9209-7b2456bca90d)


Definition_ Before_ Update class
Accept the $data parameter
First, determine whether the password is empty. If so, delete the field from the $data array. Otherwise, perform MD5 and return it to $data


![图片](https://github.com/mnbvcxz131421/douhaocms/assets/102493444/96d23099-9bad-4e3b-9b92-da724a701ade)

 

Defining Ajax_ Check_ Name is used to check if the administrator username already exists
Check separately from username and id parameters
If the username exists, return 0; otherwise, return 1


![图片](https://github.com/mnbvcxz131421/douhaocms/assets/102493444/64c34e3d-634a-489b-b420-608dd039dc1f)

 
From the above code, it can be seen that there was no verification done when adding administrators, such as adding token



recurrent

First, set up a website locally
Background ->Article Management ->Administrator Management ->Add Administrator


 ![图片](https://github.com/mnbvcxz131421/douhaocms/assets/102493444/44ff270c-d078-472f-8f50-0ede7f49f5f9)

Burp Grab Add Administrator Pack


 ![图片](https://github.com/mnbvcxz131421/douhaocms/assets/102493444/d5b9c784-e225-42fe-b68d-2647dc691b98)


Using burp to generate poc for csrf
Modify the administrator account password and other information, and delete it as. html


 ![图片](https://github.com/mnbvcxz131421/douhaocms/assets/102493444/dd6aab60-5714-4679-8904-69ccc4badddf)

Click the button to trigger the poc


![图片](https://github.com/mnbvcxz131421/douhaocms/assets/102493444/52933b9b-052c-406d-a070-f0d7ca30b261)

 
Successful operation indicates successful addition of administrator account
 
![图片](https://github.com/mnbvcxz131421/douhaocms/assets/102493444/beb8de41-8d85-4268-8eff-4312c7882a85)

Successfully logged in using the newly added administrator account 222222, password 123456
 
![图片](https://github.com/mnbvcxz131421/douhaocms/assets/102493444/26d693fc-b1fa-4184-b181-9e3bc204ca87)



Repair suggestions
Perform verification when adding an account
Add token
