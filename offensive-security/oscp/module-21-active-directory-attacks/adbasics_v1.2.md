---
coverY: 0
---

# adbasics\_v1.2

To overcome these limitations, we can use a Windows domain. Simply put, a **Windows domain** is a group of users and computers under the administration of a given business. The main idea behind a domain is to centralise the administration of common components of a Windows computer network in a single repository called **Active Directory (AD)**. The server that runs the Active Directory services is known as a **Domain Controller (DC)**.

![Windows Domain Overview](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/bebe5dfec0208bf563d01fa2dd1fb7a7.png)

The main advantages of having a configured Windows domain are:

* **Centralised identity management:** All users across the network can be configured from Active Directory with minimum effort.
* **Managing security policies:** You can configure security policies directly from Active Directory and apply them to users and computers across the network as needed.

Note: When connecting via RDP, use **`THM`**`\`**`Administrator`** as the username to specify you want to log in using the user **`Administrator`** on the `THM` domain.

```bash
rdesktop  -d THM -u \Administrator 10.10.78.52 
```

In a Windows domain, credentials are stored in a centralised repository called.

> **Active Directory**

The server in charge of running the Active Directory services is called...

> **Domain Controller**

_Security Groups_

If you are familiar with Windows, you probably know that you can define user groups to assign access rights to files or other resources to entire groups instead of single users. This allows for better manageability as you can add users to an existing group, and they will automatically inherit all of the group's privileges. Security groups are also considered security principals and, therefore, can have privileges over resources on the network.

Groups can have both users and machines as members. If needed, groups can include other groups as well.

Several groups are created by default in a domain that can be used to grant specific privileges to users. As an example, here are some of the most important groups in a domain:

| Security Group         | Description                                                                                                                                               |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Domain Admins**      | Users of this group have administrative privileges over the entire domain. By default, they can administer any computer on the domain, including the DCs. |
| **Server Operators**   | Users in this group can administer Domain Controllers. They cannot change any administrative group memberships.                                           |
| **Backup Operators**   | Users in this group are allowed to access any file, ignoring their permissions. They are used to perform backups of data on computers.                    |
| **Account Operators**  | Users in this group can create or modify other accounts in the domain.                                                                                    |
| **Domain** **Users**   | Includes all existing user accounts in the domain.                                                                                                        |
| **Domain Computers**   | Includes all existing computers in the domain.                                                                                                            |
| **Domain Controllers** | Includes all existing DCs on the domain.                                                                                                                  |

### Active Directory Users and Computers

To configure users, groups or machines in Active Directory, we need to log in to the Domain Controller and run "Active Directory Users and Computers" from the start menu:\


<figure><img src="../../../.gitbook/assets/image (374).png" alt=""><figcaption></figcaption></figure>

Which group normally administrates all computers and resources in a domain?&#x20;

> **Domain Admins**&#x20;

&#x20;What would be the name of the machine account associated with a machine named TOM-PC?&#x20;

> **TOM-PC$**&#x20;

&#x20;Suppose our company creates a new department for Quality Assurance. What type of containers should we use to group all Quality Assurance users so that policies can be applied consistently to them? &#x20;

> **Organizational Units**



### Managing Users in AD

Your first task as the new domain administrator is to check the existing AD OUs and users, as some recent changes have happened to the business. You have been given the following organisational chart and are expected to make changes to the AD to match it:

<figure><img src="../../../.gitbook/assets/image (375).png" alt=""><figcaption></figcaption></figure>

#### _Note:_ <a href="#id-0cdc" id="id-0cdc"></a>

_After the **delegation** enter the **Phillips** account by using remmina using the RDP port. Username and password: **phillip: Claire2008**_

<figure><img src="https://miro.medium.com/v2/resize:fit:1050/1*GmbbygQmEyy3bdBpQC-5Sg.png" alt="" height="294" width="700"><figcaption><p><em><strong>The RDP screen</strong></em></p></figcaption></figure>

_Inside the Phillip, account open the **Command prompt** and transfer it to PowerShell using **Command:** powershell in **cmd**._

<figure><img src="https://miro.medium.com/v2/resize:fit:957/1*zfgSlJxz9ZtrJNlyKpTH0Q.png" alt="" height="488" width="638"><figcaption><p><em><strong>The cmd to PowerShell</strong></em></p></figcaption></figure>

_Now to set the password type this command: **Set-ADAccountPassword sophie -Reset -NewPassword (Read-Host -AsSecureString -Prompt ‘New Password’) -Verbose**_

<figure><img src="https://miro.medium.com/v2/resize:fit:950/1*nFORpBIWM0ShEpbCmunA8Q.png" alt="" height="439" width="633"><figcaption><p><em><strong>The password set</strong></em></p></figcaption></figure>

_Here I have set the password: **abcD12345\***_

<figure><img src="https://miro.medium.com/v2/resize:fit:963/1*IrnP0ekWRgnX_p5Vl03TfQ.png" alt="" height="479" width="642"><figcaption><p><em><strong>Entering the account</strong></em></p></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:1050/1*RW7c6XGv4wToWs4HFwU3fw.png" alt="" height="397" width="700"><figcaption><p><em><strong>The flag</strong></em></p></figcaption></figure>

_Now we have taken the flag. So we do not want Sophie to use our given password. We will force Sophie's account to **show a reset option when Sophie will log into her account. The reset option showing process will be done from the Phillips account using this** command**: Set-ADUser -ChangePasswordAtLogon $true -Identity sophie -Verbose**_

<figure><img src="https://miro.medium.com/v2/resize:fit:990/1*X-hUb38caHLtDuHgZFoS5w.png" alt="" height="495" width="660"><figcaption><p><em><strong>Reseting the password</strong></em></p></figcaption></figure>

_Now see that you cannot enter Sophie’s account using the password you provided to get the flag. See that the password reset option has been shown._

<figure><img src="https://miro.medium.com/v2/resize:fit:995/1*tfG61-bPCvPS2u0H7l6WBw.png" alt="" height="488" width="663"><figcaption><p><em><strong>Click ok</strong></em></p></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:981/1*nMcJ7YL8kAc9xDmKOs20yA.png" alt="" height="485" width="654"><figcaption><p><em><strong>The new password option</strong></em></p></figcaption></figure>

_**The process of granting privileges to a user over some OU or other AD Object is called…**_

_**Answer:** delegation_

#### _Note:_ <a href="#id-2668" id="id-2668"></a>

_Your first task as the new domain administrator is to check the existing AD OUs and users, as some recent changes have happened to the business. You have been given the following organizational chart and are expected to make changes to the AD to match it:_

_Deleting extra OUs and users_

_The first thing you should notice is that there is an additional department OU in your current AD configuration that doesn’t appear in the chart. We’ve been told it was closed due to budget cuts and should be removed from the domain. If you try to right-click and delete the OU, you will get the following error:_

<figure><img src="https://miro.medium.com/v2/resize:fit:606/0*Xi3qkbY-6n2lDafM.png" alt="" height="159" width="404"><figcaption></figcaption></figure>

_By default, OUs are protected against accidental deletion. To delete the OU, we need to enable the **Advanced Features** in the View menu:_

<figure><img src="https://miro.medium.com/v2/resize:fit:1050/0*AIKyBL5l2pU0OTK6.png" alt="" height="388" width="700"><figcaption></figcaption></figure>

_This will show you some additional containers and enable you to disable the accidental deletion protection. To do so, right-click the OU and go to Properties. You will find a checkbox in the Object tab to disable the protection:_

<figure><img src="https://miro.medium.com/v2/resize:fit:630/0*TkvRmhQGynaCUo_6.png" alt="" height="472" width="420"><figcaption></figcaption></figure>

_Be sure to uncheck the box and try deleting the OU again. You will be prompted to confirm that you want to delete the OU, and as a result, any users, groups, or OUs under it will also be deleted._

_**After deleting the extra OU, you should notice that for some of the departments, the users in the AD don’t match the ones in our organizational chart. Create and delete users as needed to match them.**_

_**Delegation (Example: A member of an IT support group can change the username and password of the other group's low-privilege members from his account because this power is given to him by the organization using the delegate control option of the target OU in the Active directory)**_

_One of the nice things you can do in **AD** is to give **specific users some control over some OUs**. This process is known as **delegation** and allows you to grant users **specific privileges** to perform **advanced** tasks on OUs without needing a **Domain Administrator** to step in._

_One of the most common use cases for this is granting `IT support` the privilege to reset other low-privilege users' passwords. According to our organizational chart, Phillip is in charge of IT support, so we'd probably want to delegate the control of resetting passwords over the Sales, Marketing, and Management OUs to him._

### _Task 5: Managing Computers in AD:_ <a href="#id-729b" id="id-729b"></a>

_**After organizing the available computers, how many ended up in the Workstations OU?**_

_**Answer:** 7_

<figure><img src="https://miro.medium.com/v2/resize:fit:1020/1*nfIvKRKw_1KeAJO66j33gA.png" alt="" height="558" width="680"><figcaption><p><em><strong>The workstation</strong></em></p></figcaption></figure>

\
