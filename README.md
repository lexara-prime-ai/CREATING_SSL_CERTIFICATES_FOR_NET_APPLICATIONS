## Creating SSL Certificates

This repository provides a comprehensive guide and practical examples for generating **SSL (Secure Sockets Layer)** certificates tailored specifically for **.NET applications**. Whether you're building web services, APIs, or any other networked application, this resource offers step-by-step instructions and code snippets to ensure your .NET projects benefit from the added security and encryption provided by **SSL certificates**.

In case you run into issues when trying to access you dotnet application via the **browser**, the following steps should help you generate a **trusted root certificate**.

**Step 1** - Run the **api**
```powershell
PS C:\> dotnet run
```

Expected output:
![](https://github.com/projectfinalaudio/CREATING_SSL_CERTIFICATES_FOR_NET_APPLICATIONS/blob/main/run-api.png)

* If you experience any **error** that states:

```powershell
A valid HTTPS certificate with a key accessible across security partitions was not found.
The following command will run to fix it: 'sudo security set-key-partition-list -D localhost -S unsigned:,teamid:UBF8T346G9'
This command will make the certificate key accessible across security partitions and might prompt you for your password.
For more information see:  [https://aka.ms/aspnetcore/2.1/troubleshootcertissues](https://aka.ms/aspnetcore/2.1/troubleshootcertissues)

A valid HTTPS certificate with a key accessible across security partitions was not found.
The following command will run to fix it: 'sudo security set-key-partition-list -D localhost -S unsigned:,teamid:UBF8T346G9'
This command will make the certificate key accessible across security partitions and might prompt you for your password.
For more information see:  [https://aka.ms/aspnetcore/3.1/troubleshootcertissues](https://aka.ms/aspnetcore/3.1/troubleshootcertissues)

Trusting the HTTPS development certificate was requested.
A confirmation prompt will be displayed if the certificate was not previously trusted.
Click yes on the prompt to trust the certificate. There was an error trusting HTTPS developer certificate.
```
* Run the following commands:
```powershell
PS C:\> dotnet dev-certs https --clean
PS C:\> dotnet dev-certs https --check
```

```scss
// (DO THE MANUAL keychain-old-localhost removal NOW (image below) 
// (before running the next commands)
```
![manual-removal]()
![manage-certs]()
![remove-old-certs]()
![remove-old-root-certs]()

```powershell
PS C:\> dotnet dev-certs https --check
PS C:\> dotnet dev-certs https 
PS C:\> dotnet dev-certs https --trust
```

Expected output:

![generating0new-certs](https://i.stack.imgur.com/0049R.png)

**Step 2** - Setup '***appsettings.Development.json***'

Create a .**json** file and add the following configuration. It contains the connection string the .NET application will use to connect to an existing database instance as well as properties that will be used for generating and validating Json Web Tokens.
```json
{  
	"Logging": {  
		"LogLevel": {  
			"Default": "Information",  
			"Microsoft": "Warning",  
			"Microsoft.Hosting.Lifetime": "Information"  
		}  
	},  
	"ConnectionStrings": {  
		"DefaultConnection": "Server=localhost,14331;Database=db;UserId=sa;Password=cshfdfhfd^*&%&34214dfsdgsdADD;TrustServerCertificate=true;"  
	},  
	"Token": "thisIsMyToken!DareYou2HackItButYouJustCan't",  
	"AllocationsToken": "thisIsMyToken!DareYou2HackItButYouJustCan't",  
	"AllowedHosts": "*",  
	"SendGrid": {  
		"APIKey": 	"thisIsMyToken!DareYou2Hqi3rryqw8o3ryqweifhsdhfqewiryoqwheuhqewuirhqweiurqweirqweackItButYouJustCan't"  
		},  
	"UI": {  
	"URL": ""  
	}  
}
```

**Step 3** - **Restart** the api and **re-load** the **swagger endpoint** in your browser
```powershell
PS C:\> dotnet run
```

The **.NET application** should run *without* any errors.
