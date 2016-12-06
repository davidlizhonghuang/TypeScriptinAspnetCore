# TypeScript CRUD operations in Aspnet Core

Asp.net core, next generation of web application, uses TypeScript now. see <a href="https://www.typescriptlang.org/#download-links">link</a> here.
Asp.net core has built all necessary environment for developer to develop typescript as javascript's way. we do not worry about the gulp,task runner,etc.

We simply create a folder under "wwwroot" folder called ts.
Then we add a new typescript file from Visual Studio 2015 called app.ts.
Save file we will see not many changes as we create a new javascript file.
Open app.ts file and add some typescript codes in and save it again, we can see a new app.js file is generated under this app.ts file when you collapse this ts file.
Use require js to load this app.js in html page.
We can simply see the result from typescript scripting.

Typescript is an OO programming language.We can create a module, like the namespace in .net, and import this namespace into ts file, we  can create interface and classes in module.
We can simply create a javascript function to call class and function in this typescript. 
After typescript is created and saved, a new js file is created automatically. 
Typescript can use jquery and interact with the element and selector in html page via native javascript and jquery.
Typescript allows us to inject third party typescript files as reference.

Asp.Net Core uses typescript as js front end programming platform. After we use typescript, we may will not be willings to move back to javascript. I have tasted some sugar from using this new js technique in asp.net core. This demo project will show you how to get used to this new technique in asp.net core web site.

##Asp.net Core

Asp.net core allows us to create mvc controllers, mvc api controlers, razor view, html page for multiple platofrm web pogramming. Its startup file is so powerful that we can easily inject resources we need in this core web application. We inject httpcontext error handler as a middleware , we register logger  and  inject third party data sources in this startup file. We build environment in appsettings.json file , so we can get appsetting key/value pairs and connection strings, etc from this json file to maintain the application level data. We add security policy definitions in startup file so we can protect web services in backend and stop cross domain calls and more such as Authentication,CORS,Routing,Session,Routing, and Diagnostics, etc. We set up middleware (classic httpmodule/httphandler in asp.net) in this startup file to handle request and response via Run Map Use.

We can ignore asp.net MVC pattern in core and simply use html page in wwwroot folder, html page will use js framework to do CRUD operations in server side. This allows us to develop a light weight single web page application quickly. in startup middleware, we need to add app.useidentity after staticfile to make sure wwwroot is accessed by public.

Asp.net Core's power is its capability of developing loose coupled enterprise based responsive web applications for modern multiple devices cross platforms such as Apple IOS. I have developed an internal multiple tier Tea Inventory project in asp.net core by using anguar js 1.5, httpclient web api proxy, wep api 2, and asp.net core entity frameworks as well as SQL server 2016. Now I want to show you how to do typescript pogramming in asp.net core web application in an easy way.

##TypeScript in Asp.net Core

TypeScript can be installed in various ways, VS 2015 has taken typescript as a default js programming in vs 2015 asp.net core. Typescript 2 now is available, namespace is invented to represent internal module and module now can be the external module.
TypeScript is module design pattern programming language. We develop different modules that we added them as a reference to be used in different instance classes. The programming is as easy as C# if you know C#. The following steps are briefly documented how to develop a demo typescript CRUD operations in Asp.net Core Tea Inventory web application.

###1, Create a new asp.net Core web app

Open VS 2015, select new project , web, and asp.net core web application to create a new asp.net web core application.
Create a new folder called api under controllers folder, Add a new controller called SlotsController.cs.
Create a folder called Slots under views folder, add a new item called index.cshtml in this folder.
Add attributes at the top of the controller as below
<pre>

    [Route("api/Slots")]
    [EnableCors("CorsPolicy")]
    [Authorize(Policy = "adminpolicy")]
public class SlotsController : Controller
{
private readonly InventoryContext _context;        	
private ILogger<SlotsController> _logger;
public SlotsController(InventoryContext context, ILogger<SlotsController> logger)
{
            _context = context;
            _logger = logger;
 }
.......
 }
 </pre>
 
Asp.net Core is  the .net web development platform with dependency injection automatically enabled, so we can simply inject the data source InventoryContext and microsoft logger service in API controller easily, we do not need to download third party dependency injection IoC containers. Here, _context is the data source we created in asp.net core Entity Framework data access layer for this web application.
 
Create Get,  Put, Delete,  and Post actions in this controller as the example below. 
<pre>
        [HttpPost]
        public async Task<IActionResult> Post([FromBody] Slot slot)
        {
            if (ModelState.IsValid)
            {
               _context.Slots.Add(slot);
                _logger.LogInformation((int)1, "Add slot to database");
                await   _context.SaveChangesAsync();          
            }
             return  Json("done");
        }
        [HttpGet]
        public async Task<JsonResult> Get()
        {
            List<Slot> result = new List<Slot>();       
            if (ModelState.IsValid)
            {
                IQueryable<Slot> rtn = from temp in _context.Slots select temp;
                _logger.LogInformation((int)2, "get slot from database");
                result = rtn.ToList();
            }
            return  Json(result);
        }
        public int GetnewId()
        {
            int id = 0;
            if (ModelState.IsValid)
            {
                if (_context.Slots != null)
                {
                    if (_context.Slots.Count() > 0)
                    {
                        IQueryable<Slot> rtn = from temp in _context.Slots select temp;
                        _logger.LogInformation((int)3, "get new id slot from database");
                       id = rtn.ToList().Max(x => x.SlotNo) + 1;
                    }
                    else
                    {
                        id = 1;
                    }
                }
                else
                {
                    id = 1;
                }
            }
            return id;
        }

        [HttpGet("{id:int}")]
        public async  Task<JsonResult> Get(int id)
        {
            Slot result = new Slot();
            if (ModelState.IsValid)
            {
                result = _context.Slots.FirstOrDefault(c => c.SlotNo == id);
                _logger.LogInformation((int)4, "get each slot from database");
            }
            return Json(result);
        }

        [HttpPut]
        public async Task<IActionResult> Put([FromBody] Slot jslot)
        {
            var result= _context.Slots.FirstOrDefault(c => c.SlotNo == jslot.SlotNo);
            if (ModelState.IsValid)
            {
                result.SlotNo = jslot.SlotNo;
                result.SlotName = jslot.SlotName;
                result.Description = jslot.Description;
                _context.Slots.Attach(updateSlot);
                _context.Entry(updateSlot).State = System.Data.Entity.EntityState.Modified;
               _logger.LogInformation((int)6, "update slot from database");
                await _context.SaveChangesAsync();
            }
            return Json("done");
        }

        [HttpDelete]
        public async Task<IActionResult> Delete(int id)
        {
            if (ModelState.IsValid)
            {
                var result= _context.Slots.FirstOrDefault(x => x.SlotNo == id);
                _context.Entry(updateSlot).State = System.Data.Entity.EntityState.Deleted;
                _logger.LogInformation((int)2, "delete slot from database");
              await  _context.SaveChangesAsync();
            }
            return Json("done");
        }

</pre>
Now an Asp.net web api has been created for front end developent.

##2, TypeScript Front End development
At moment, we forget about angular js. We use built in typescript in asp.net core for front end development.
Create a folder called ts under wwwroot folder and add a new item , select typescript and create a new app.ts file under this new folder
Add  below code to app.ts 
<pre>
module slotlocal
{
    interface Islot {
        id: number;
        name: string;
        desc: string;
        SelectSlot(callback:any);
    }
    export class Slot implements Islot {
        public id: number;
        public name: string;
        public desc: string;
        public SelectSlot(callback: any): void {
            $.ajax({
               method: "GET",
                url: "AllSlots"
            }).then(callback, function (err) {
                    console.log(err);
            });
        }
    }
}

function searchslot() {
    let slotall = new slotlocal.Slot();
    slotall.SelectSlot(callbacka);
}

function callbacka(data)
{
    $.each(data, function (key, val) {
        var tableRow = '< tr>' +
            '< td>' + val.slotNo + '< /td>' +
            '< td>< input type="text" value="' + val.slotName + '"/>< /td>' +
            '< td>< input type="text" value="' + val.description + '"/>< /td>' +
            '< td>< input type="button" name="btnUpdate" value="Update" /> < input type="button" name="btnDelete" value="Delete" />< /td>' +
            '< /tr>';
        $('#customerTable').append(tableRow);
    });
}
</pre>


Here, we simple define a module that actually is a namespace. In this module, we add an interface and class that can be exported (can be seen in public). We enable Jquery in typescript and then we embed $.ajax call in typescript, This section programming is similar to C#.
Then we create a function to consume this class in typescript.  Function in TypeScript can access DOM directly to send or get data from DOM. W call function here is a middle tier. DOM is front end view, and typescript is the backend. This middle tier glue DOM view and typescript class together. So you forget about javascript here.DOM can call this function directly. This function is the javascript function in typescript. It plays key role in exposing typescript class to DOM.
Now in Razor cshtml page add some html controls in as example below
<img src="xsc1.png">
After Click search button , we can see two records are displayed as below
<img src="xsc.png">
### 3, Summary
Now we can see the typescript in vs 2015 asp.net core can be used to develop CRUD operations in asp.net core application easily. 




