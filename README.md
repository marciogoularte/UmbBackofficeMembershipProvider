# UmbBackofficeMembershipProvider
Code to allow Umbraco 7.3.1+ to use MembershipProvider-based providers for Active Directory authentication.

## What's inside
This project includes a DLL that will allow you to use a traditional `MembershipProvider` for logging in Umbraco backoffice users.

## System requirements
1. NET Framework 4.5
2. Umbraco 7.3.1+

# NuGet availability
This project is available on [NuGet](https://www.nuget.org/packages/UmbBackofficeMembershipProvider/).

## Usage instructions
### Getting started
1. Add **UmbBackofficeMembershipProvider.dll** as a reference in your project or place it in the **\bin** folder.
2. Add the dependency [**UmbracoIdentityExtensions**](https://github.com/umbraco/UmbracoIdentityExtensions) as a reference in your project or place its DLL in the **\bin** folder.
3. In **web.config**, make the following two modifications:
  - Add or modify the following line in the `<appSettings>` section:

    ```
    <add key="owin:appStartup" value="BackofficeMembershipProviderCustomOwinStartup" />
    ```
  - Add a LDAP connection string to your LDAP server in the `<connectionStrings>` section, like shown in the example code below. Specify a path to the domain root or a container/OU if you want to limit where the user accounts can be located.
  
    ```
    <add connectionString="LDAP://mydomain.mycompany.com/DC=mydomain,DC=mycompany,DC=com" name="ADConnectionString" />
    ```
  - Add a membership provider named `BackofficeMembershipProvider`, like shown in the example code below. Be sure the `connectionStringName` matches the LDAP connection string you defined. `attributeMapUsername` specifies the username format - `sAMAccountName` for just the username, or `userPrincipalName` to use username@mydomain.mycompany.com. Be sure the usernames you configure in Umbraco use the same format.
  
    ```
    <membership defaultProvider="UmbracoMembershipProvider">
      <providers>
        <add
           name="BackofficeMembershipProvider"
           type="System.Web.Security.ActiveDirectoryMembershipProvider, System.Web, Version=2.0.0.0, 
                 Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a"
           connectionStringName="ADConnectionString"
           attributeMapUsername="sAMAccountName"
           connectionUsername="testdomain\administrator" 
           connectionPassword="password"/>
      </providers>
     </membership>
 ```
 
### User accounts
In versions of Umbraco before 7.3.0, Umbraco automatically creates Umbraco user accounts for Active Directory users on first login. In versions 7.3.0 and newer, an administrator must create an Umbraco user account (use the same username) first before an Active Directory user can login.
