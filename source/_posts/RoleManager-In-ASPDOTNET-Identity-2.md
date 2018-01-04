title: RoleManager in ASP.NET Identity 2.0
date: 2015-11-02 08:02:49
tags: [ASP.NET MVC, Identity]
categories: [.NET, ASP.NET MVC]
---
ASP.NET Identity 2.0 has been included in the standard template since Visual Studio 2013.
It's available in MVC and WebAPI as ONE ASP.NET.

## Where is Role?
Role based access control has managed user access since Form Authentication but it's missing in the current template.
Just adding some code snippets to the template as follows in order to enable this.


### Models/IdentityModels.cs
<pre class="brush: c-sharp;">
public class ApplicationRole : IdentityRole
{
}
</pre>

### App_Start/IdentityConfig.cs
<pre class="brush: c-sharp;">
public class ApplicationRoleManager : RoleManager&lt;ApplicationRole, string&gt;
{
    public ApplicationRoleManager(IRoleStore&lt;ApplicationRole, string&gt; store)
        : base(store)
    {
    }
    public static ApplicationRoleManager Create(IdentityFactoryOptions&lt;ApplicationRoleManager&gt; options, IOwinContext context)
    {
        var dbContext = context.Get&lt;ApplicationDbContext&gt;();
        var roleStore = new RoleStore&lt;ApplicationRole&gt;(dbContext);
        var manager = new ApplicationRoleManager(roleStore);

        // Add some roles (e.g. "Administrator") if needed
        if (!manager.Roles.Any(r =&gt; r.Name == "Administrator"))
        {
            manager.Create(new ApplicationRole
            {
                Name = "Administrator"
            });
        }
        return manager;
    }
}
</pre>

### Models/IdentityModels.cs
<pre class="brush: c-sharp;">
public partial class Startup
{
    public void ConfigureAuth(IAppBuilder app)
    {
        app.CreatePerOwinContext(ApplicationDbContext.Create);
        app.CreatePerOwinContext&lt;ApplicationUserManager≥(ApplicationUserManager.Create);

        // Add this line
        app.CreatePerOwinContext&lt;ApplicationRoleManager&gt;(ApplicationRoleManager.Create);
    }
}
</pre>

### Controllers/AccountController.cs
<pre class="brush: c-sharp;">
public class AccountController : Controller
{
    private ApplicationUserManager _userManager; 
    private ApplicationRoleManager _roleManager;

    public AccountController()
    {
    }

    public AccountController(ApplicationUserManager userManager, ApplicationRoleManager roleManager, ApplicationSignInManager signInManager)
    {
        UserManager = userManager;
        RoleManager = roleManager;
        SignInManager = signInManager;
    }

    public ApplicationRoleManager RoleManager
    {
        get
        {
            return _roleManager ?? HttpContext.GetOwinContext().Get&lt;ApplicationRoleManager&gt;();
        }
        private set
        {
            _roleManager = value;
        }
    }
}
</pre>

If DB is existing, it's need to be renewed.
Execute the following commands in Package Manager

<pre class="brush: bash;">
PM&gt; Enable-Migrations
PM&gt; Add-Migration "AddRole"
PM&gt; Update-Database
</pre>
