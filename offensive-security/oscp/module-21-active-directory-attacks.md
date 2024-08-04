# Module 21 (Active Directory Attacks)

### AD OverFlow

Active directory is just like a phone book, Basically used to store series of information in relation to what is called an object , could be printers, computers, users etc

AD also serves as an identity management service in the sense that, take for example you change your password in a particular system then all systems in that particular network, password's, get changed too, so now you change your password on one system and you can still login on other system with that same password

In active directory Authentication happens with kerberos or kerberos ticket

![](https://i.imgur.com/3PA0Ogb.png)

Active directory is commonly used in banks and 95% of fortune 1000 companies and the major reason why it is vulnerable in the wild are due to misconfigurations, bad setup etc

![](https://i.imgur.com/ihBjo17.png)

### Physical Active Directory Components

![](https://i.imgur.com/r8LCjYP.png)

1. Domain Controllers: A domain controller is a type of server that processes user requests for authentication within a computer [domain](https://www.techtarget.com/whatis/definition/domain). Domain [controllers](https://www.techtarget.com/whatis/definition/controller) are most commonly used in Windows Active Directory ([AD](https://www.techtarget.com/searchwindowsserver/definition/Active-Directory)) domains but are also used with other types of identity management systems.

![](https://i.imgur.com/iOUQHz2.png)

2. AD DS Data Store : Contain database files for users, services and applications also stores information about user accounts, such as names, passwords, phone numbers, and so on, and enables other authorized users on the same network to access this information. it also stores a very important file called **Ntds.dit**

![](https://i.imgur.com/Jzd7UAu.png)

### Logical Active Directory Components

![](https://i.imgur.com/r8LCjYP.png)

1. AD DS Schema : You can think of it as a rule book or a blue print, which defines every type of object that can be stored within our directory (Objects are normally defined as either resources, such as printers or computers, or security principals, such as users or groups.)

![](https://i.imgur.com/kdCAfUs.png)

2. Domains : could be defined as a collection of objects within a Microsoft Active Directory network. an example of a domain could be google.com, hackersploit.org etc, we need where to manage our objects (Printers and computers) that is where the domain comes in

![](https://i.imgur.com/xcPSX8N.png)

3. Trees : A tree is a hierarchy of domains, Just like subdomains could be drive.google.com, question.hackersploit.org

![](https://i.imgur.com/y9TzJet.png)

4. Forests : a forest is a set or collection of trees in an active directory

![](https://i.imgur.com/pigpqyr.png)

5. Organizational Units (OU's) : is a subdivision within an Active Directory into which you can place users, groups, computers, and other organizational units. They represent functional or ad-hoc groupings, such as committee, a task force, a project management organization, a class (for education) and so on. also objects lived within an OU

![](https://i.imgur.com/4jvmYHH.png)

6. Trusts : Is a method of connecting two distinct Active Directory domains (or forests) to allow users in one domain to authenticate against resources in the other.

![](https://i.imgur.com/cBcAsLZ.png)

7. Objects : An object is **a single element, such as a user, group, application or device such as a printer**. Objects are normally defined as either resources, such as printers or computers, or security principals, such as users or groups.

![](https://i.imgur.com/2aWO5mU.png)
