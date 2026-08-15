
# Access Control= Rules that decide who can do what.
mainly there are 3 types: Vertical, Horizontal and context-based
1. vertical Access Control 
Controls access based on role or privilege level. Higer role =more permissions.
• Broken vertical Access Control→ When customer visits a (website.com/admin) and suddenly sees the admin dashboard.  Huge Vulnerability.

2. Horizontal Access Control
  People with the same role should not see each other's data. id change one where if u change id.

3. Context-Dependent Access Control
This depend on time, state or situation. eg. Airline: Seat booking and every one booked same seat without understand the context which is a vul. In Bank pass reset link valid 10 min and after that it should expire.
☐ Broken Access Control: System fail to enforce access restrictions correctly.
      ‘’SO 1st lab unprotected admin 
      SImply i did ctrl + u and inspect page there was a js script and there was /admin-6c8nme which was needed and i paste it in url and i deleted carlos''
     
4. Parameter-based access control methods
    Some applications determine the user's access rights or role at  login, and then store this information in a user-controllable location.  This could be: A hidden field, a cookie or a present query string parameter.
“This lab was fucking hell i did login and then capture it in proxy history and send it to repeater and did admin to ture  and then send it after that inspect to change admin form false to ture”
“SO i solved this lab ”user role can be modified in user profile" one it was easy but the previous day i tried to solve but i couldnot for an hour i simply loged  in using the given ids and then update the email and then capture it in http history then i send it to repeater initially i sent it and i got the response roleid:1. Now i added “roleid”:2 and send it and boom it worked but here before adding script down i forget to put comma(,) and that was the error.

5. BROKEN ACCESS CONTROL RESULTING FROM PLATFROM MISCONFIGURATION
 → they do this by restriciting access to some urls and HTTP method based on the user's role.
 an example:DENY: POST, /admin/deleteUser, managers  here access to post is denied by /admin/de... for user in manager group.
 → URL types: X-Original -URL and X-rewrite-URL .  Even if the fornt end standard (rigorous) is maintained but allows URL overridden via a req header then it might be possible to bypass request using like this:  POST / HTTP/1.1X-Original-URL: /admin/deleteUser.. 
# ☑  SO i couldn't solve X-Original-URL one  so i had to look the soln and i didn't know how to add those querry even that i did no success as i added X-Original-URL:/invalid one in 1 st line in GET request so it didn't work and after i watched the community soln i got to know it to be added after /net line and there should not be a parameter which i did the mistake of adding /admin/delete?username=carlos but after ? it has to be added in 1st line i did't know and before doing this stuff first after testing /admin in x original we should right click in the req/hex page and open this in a browser copy and open that page in the browser to see the 2 user and delete carlos initially access denied ofc and after that use the query i mentioned earlier using the right parameter in right place.

# ☑ Unprotected functionality
Simply server fails to verify who is allowed to view or use sensitive or privileged parts of a website. → Can be exploited if path left in publicly accessible files like robots.txt. Or Directory brute forcing like /admin, /administrator to discover hidden path  or UI Exposure.
TO STOP / FIX IT :- Enforce Authentication, Implement Role-Based Authorization, Stop Relying on Obscurity, Sanitize code.
• SO i solved this level by simply changing the url to robots.txt and in proxy tab in burp after that i send it to repeater → i changed that to administrator-panel and then i tried to delete like i did previosly /admin.../delete/?user.. one it didn't work  so i simply wnet back to administrator-panel and send it and copy that and i opened it in browser bingo and then i deleted carlos.
• METHOD-BASED ACCESS CONTROL CAN BE Circumvented
Technically i completed this lab within a minute using intercept on and changing the request parameter and giving my self a admin privilage but this level require a clear exploit as it follows steps i.e. S1→ log in using admin and upgrade carlos to admin step 2→ see that request in proxy and send it to repeater step 3 log out and login using the given user id step 4 see the login req in proxy and then copy the session cookie step 5→ go to repeater and change the session cookie it's a post req so u will see and error so change the request clicking right and change after that change id username form carlos to given username(wiener) . Level completed.
# USER ID controlled by request parameter
Here just login with given id and then in proxy see and send to repeater and then change the name form wiener to carlos that's it.
GUID ( Globally Unique identifier) is a long 36 character text code like (123e4567-e89b-12d3-a456-426614174000)  used by computers to uniquely name digital items. Fun fact: Two separate computer can make a GUID at the exact same time without ever matching. V1 old GUID which uses computer network.
#USER ID CONTROLLED BY REQUEST PARAMETER, WITH PREDICTABLE USER IDS
So basically browse the lab and see carlos also see the request in brup there u will see his user id copy it and try to log in using the given credentials. Send the loged in credentials to Repeater and place the user id with carlos after that Request in browser original session and paste that url and bingo u got the API key.
#USER ID CONTROLLED BY REQUEST PARAMETER WITH DATA LEAKAGE IN REDIRECT
In this lab login in with the given credentials and see that req in burp after that send that req containing id to repeater now change the id name to carlos then send now get the API key in response.
☐ HORIZONTAL TO VERTICAL PRIVILEGE ESCALATION
Simply by compromising a more privilege user. An attacker might be able to gain access to another user's account page using the parameter tampering technique already described for horizontal privilege escalation.
•# USER ID CONTROLLED  BY REQUEST PARAMETER WITH PASSWORD DISCLOSURE
So login using the given credential after that change the id to administrator then see the response thier is the password now login to administrator and delete. If u just request in the browser, copy and then open u won't see admin panel.
*#* IDOR( Insecure direct  object reference):- Sub category of access control vulnerabilities. IDORs occur if an application uses user-supplied input to access objects directly and an attacker can modify the input to obtain unauthorized access.
# IDOR
To solve this lab select live chat, send a message and then select view transcript. Review that url in burp as it contain test file with incrementing number now change the file name to 1.txt and review the text. 
• More about idor:Consider the following url to access the customer account page: https://insecure-website.com/customer_account?customer_number=132355 Here customer nuber is directly used as a record index in quries that are performned on the back-end database. If no other control are in place, an attacker can simply modify customer_number value, bypasssing access control to view the records of other customers. IDOr vul leading to horizontal privilege escalation.An attacker might be able to perform horizontal and vertical privilege escalation by altering the user to one with additional privileges while bypassing access controls. Other possibilities include exploiting password leakage or modifying parameters once the attacker has landed in the user's accounts page, for example.  IDOR vulnerability with direct reference to static files→ Arise when sensitive resources are located in static files on the server-side filesystem.  For example, a website might save chat message transcripts to disk  using an incrementing filename, and allow users to retrieve these by  visiting a URL like the following:      https://insecure-website.com/static/12144.txt 
→ In this situation, an attacker can simply modify the filename to retrieve a transcript created by another user and potentially obtain user credentials and other sensitive data. 
☐ Access control vulnerabilities in multi-step processes
Many website implement important functions over a series of steps.
• A variety of inputs or options need to be captured.
• The user needs to review and confirm details before the action is performed.  
For example: administrative function to update user details might involve these steps:
1. Load the form that contains details for a specific user.
2. Submit the changes.
3. Review the changes and confirm.
Sometimes, a website will implement rigorous access controls over some of these steps, but ignore others. Imagine a website where access controls are correctly applied to the first and second steps, but not to the third step. The website assumes that a user will only reach step 3 if they have already completed the first steps, which are properly controlled. An attacker can gain unauthorized access to the function by skipping the first two steps and directly submitting the request for the third step with the required parameters.
# Multi-step process with no access control on one step:
So in this lab login in using the given admin credential after that promote the user carlos to admin then see the 2 request in burp and send it to repeater. Now logout and login in using normal given credential i.e wiener one. Inspect and get the cookie of wiener. After that change the cookies in both request and send one will show error and another will not show error the one that doesn't show error change that name from carlos to wiener. Here the one that donot show error is the one that is sending the conformation request as we tried to make carlos admin so that conformation request is the one.
# • Referer-based access control
Some websites base access controls on the Referer header submitted in the HTTP request. The Referer header can be added to requests by browsers to indicate which page initiated a request. For example, an application robustly enforces access control over the main administrative page at /admin, but for sub-pages such as /admin/deleteuser only inspects the Refer header. If the Refer header contains the main /admin URL, then the request us allowed.
In this case, the Referer header can be flly controlled by an attacker. this means that they can forge direct requests to sensitive sub-pages by supplying the required Referer header, and gain unauthorized access. 
## REFERER-BASED ACCESS CONTROL
This lab is also same just login using admin credentials, promote carlos to admin after that see and send that request to repeater now login using normal credential i.e wiener one and get the cookie of wiener and again in admin conformation request change the cookie and name.
☐ 
Location-based access control
   Some websites enforce access controls based on the user's  geographical location. This can apply, for example, to banking  applications or media services where state legislation or business  restrictions apply. These access controls can often be circumvented by  the use of web proxies, VPNs, or manipulation of client-side geolocation  mechanisms.  
  
☐ How to prevent access control vulnerabilities
Access control vulnerabilities can be prevented by taking a defense-in-depth approach and applying the following principles:  
1. Never rely on obfuscation alone for access control.  
2. Unless a resource is intended to be publicly accessible, deny access by default.
3. Wherever possible, use a single application-wide mechanism for enforcing access controls.
4. At the code level, make it mandatory for developers to declare the access that is allowed for each resource, and deny access by default.  
5. Thoroughly audit and test access controls to ensure they work as designed.
