Site: http://student-companion.tk/

Website for Student Companion problem. Ahmedabad University Programming Club.

<h1>Please note while judging!!:</h1> 

- Many of the conventional "best practices" associated with react and other libraries (such as not importing too many libraries, not using inline functions, etc.) may not apply to svelte as it is a compiler, not a framework. It intelligently compiles the svelte code into js, html, and css, applies treeshaking to libraries (only using the parts required), and optimizes the code for best performance and smallest bundle.js sizes possible. That also means that conventional known "bad practices" may not apply to svelte as it can compile the code and not face performance losses (for certain practices) where other non-compiled traditional frontend frameworks would.


- Currently I have enabled anyone to be an admin so that it is easier to judge the admin panel. I have already coded the rules for firebase database security so that no one but the admin can make changes to the courses, but those are disabled as of now so that any judge can login and view the admin panel. 

- Login from facebook, github, and microsoft accounts is not enabled yet as it requires hosting/registering the app on the respective site, which is a long process for some sites. **Google login, email login, and sign in as guest work as intended.**


- Most of the bundle.js size for this project comes from the code/libraries for the firebase API.
