# Creating and Managing Users
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Creating and Managing Users with Active Directory</h1>
This tutorial outlines creating and managing users using Active Directory .<br />


- Microsoft Azure (Virtual Machines/Computer)
- Remote Desktop
- Active Directory Domain Services
- Group Policy

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Creating and Managing Users Steps</h2>

- Step 1: Set up Remote Desktop for non-administrative users on Client-1
- Step 2: Create a non-administrative user 
- Step 3: Setting up account lockout
- Step 4: Testing account lockout

<h2>Set up Remote Desktop for non-administrative users on Client-1</h2>
<p>
Log in to Client-1 as mydomain.com\jane_admin and open up the about page again. Click on Remote Desktop, and then we need to allow domain users to access Remote Desktop. Under User accounts, click "Select users that can remotely access this PC". In the new pop-up window, click Add and then type in Domain Users, and click okay. We can now log in to Client-1 as a non-admin user, but we will need to create one first! So let's sign out of Client-1 and hop back into DC-1.
</p>

![image](https://github.com/user-attachments/assets/2f956696-ac2d-4811-9f81-3526ab381043)

<br />

<h2>Create a non-administrative user</h2>
<p>
Now that we are back in DC-1, let's make sure we open up Active Directory Users and Computers. In the _Employees folder, let's right-click and select New User. We can do any name, but I did John Smith. Click next, put a password in for me that's Azurelab123!. Click Next and finish.
  
</p>
<br />

![image](https://github.com/user-attachments/assets/146d5920-83c4-4983-8f7b-1d92685d95a0)
![image](https://github.com/user-attachments/assets/27ac1ba5-2909-4acb-8e34-4921650de4e6)


<p>
The newly created user will already be a part of Domain Users. We can check this by right-clicking our user and then clicking properties. From here, select Member Of and you will see that they are a Domain user.

![image](https://github.com/user-attachments/assets/0927fa5e-d57e-47a2-a1ea-51ef8df5f8b7)
</p>
<h2>Setting up account lockout</h2>
<p>Now let's go ahead and use Group Policy to add account lockout. This will make it so that if John enters their password too many times incorrectly, they will be locked out of being able to log in and will either need to wait or contact an administrator. So, still in our Domain Controller, DC-1, click Start and type gpmc.msc and hit Enter. This will bring up the Group Policy Management Console. From here, we will want to open up our forest, mydomain.com, and let's select the dropdown for mydomain.com, and then you'll see that there is one already attached labeled Default Domain Policy. Right-click that and select edit
</p>

![image](https://github.com/user-attachments/assets/eb57fe44-51fa-40c8-b838-83e99e4dbd2b)

<p>
Next, we will click the drop-down for Computer Configuration, Policies, Windows Settings, Security Settings, Account Policies, and Account Lockout Policy. From here, we can edit the settings in the menu on the right. Let's set the Account Lockout threshold Properties to 5 by double-clicking it and then setting its number to 5. GP will use some logic to auto-set the other properties, and we could change those to our specifications if we would like.
</p>

![image](https://github.com/user-attachments/assets/a2b21406-c968-495a-8e72-aa69a94858e8)

<p>
We need to update Group Policy on Client-1 so that this rule is in place. Back in Client-1 as Jane admin, we will want to run Command Prompt as an administrator, type gpupdate /force, and hit Enter. We can then run gpresult /r to see when it was last applied.

</p>

![image](https://github.com/user-attachments/assets/2b9dd398-9e3c-450e-8b09-b6036a72ceba)

<h2>Testing account lockout</h2>
<p>
Now that account lockout has been set up we can test this by signing out of Client-1 and signing in as our new user John that we had created, but make sure you enter the INCORRECT password 5 times. If you do so, you should get this message to appear.

![image](https://github.com/user-attachments/assets/ff6b9b59-7d03-4577-b0b4-ea0892d9f285)

We could wait the 5 minutes that were set to enter a new password, but we don't need to! If we sign back into DC-1 as an admin, we can go ahead and remove the account lockout. In Active Directory Users and Computers right click the user and go to properties. Select account and then we can Check the checkbox for Unlock account. This will allow left the account lock off of john's account so that they can sign in with out having to wait. 

![image](https://github.com/user-attachments/assets/00627167-052e-45b0-9918-d76fc1c8cf0f)

If we go back, you can now see that we can log in as John Smith.
  
</p>

![image](https://github.com/user-attachments/assets/08895cc3-a9a8-4466-812c-630c16260fd0)

<p>
You can do so many things with GP, pretty much anything you can think of, but we are done with these machines for the scope of the activity. Go ahead and delete the resource group along with the VM if you are all done, or if you would like, feel free to play around to learn more in the new environment that you created.
</p>
