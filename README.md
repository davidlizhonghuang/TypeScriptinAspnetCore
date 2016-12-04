# TypeScript in Aspnet Core

Asp.net core, next generation of web application, uses TypeScript now. see <a href="https://www.typescriptlang.org/#download-links">link</a> here.
Asp.net core has built all necessary environment for developer to develop typescript as javascript's way. we do not worry about the gulp,task runner,etc.

We simply create a folder under "wwwroot" folder called ts.
Then we add a new typescript file from Visual Studio 2015 called app.ts.
Save file we will see no much changes as we create a new javascript file.
Open app.ts file and add some typescript code in and save it again, we can see a new app.js file is generated under this app.ts file when you collapse this ts file.
Use require js to load this app.js in html page.
We can simply see the result from typescript scripting.

##Typescript is a OO programming language

We can create a module, like the namespace in .net, and import this namespace into ts file, we then can create interface and classes in module.

###The one beauty of typescript is we can simply create a javascript function to call class and function in this typescript. 

After typescript is created and saved, a new js file is created automatically. 

###The another beauty of typescript is that typescript can use jquery and can interact with the element and selector in html page via native javascript.

Typescript allows us to inject third party typescript files as reference.

Asp.net Core can integrate typescript in razor view very well as this demo indicated. Have a fun.
