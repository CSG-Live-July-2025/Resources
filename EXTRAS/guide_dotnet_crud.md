# Guide: .NET API CRUD practice

## Install .NET and create a new API app
1.  Start with a clean slate

    - Close all your text editor windows, quit your text editor.
    - Find and kill any terminal servers using Ctrl C.
    - Open Terminal and navigate to your Actualize folder.

2.  Install .NET SDK (if not already installed):

    ```
    brew install --cask dotnet-sdk
    ```
    
    > Verify installation by running `dotnet --version`

3.  Set up the environment variable (required on some Macs):

    ```
    echo 'export DOTNET_ROOT=/usr/local/share/dotnet' >> ~/.zshrc
    source ~/.zshrc
    ```

4.  In your terminal, create a new .NET Web API project:

    <pre><code>dotnet new webapi -n <ins><strong>photo-api</strong></ins></code></pre>

5.  In the terminal, go to your new directory and restore packages:

    <pre><code>cd <ins><strong>photo-api</strong></ins></code></pre>
    
    ```
    dotnet restore
    ```
    
    > This downloads NuGet packages and makes dependency errors more obvious

6.  In the terminal, install Entity Framework packages:

    ```
    dotnet add package Microsoft.EntityFrameworkCore.Sqlite
    dotnet add package Microsoft.EntityFrameworkCore.Design
    ```

7. In the terminal, install Swagger packages (newer .NET templates use OpenAPI by default):

    ```
    dotnet add package Swashbuckle.AspNetCore
    ```

8. In the terminal, create the Controllers directory:

    ```
    mkdir Controllers
    ```

9. Update your **Program.cs** to use controllers instead of minimal APIs:

    ```csharp
    var builder = WebApplication.CreateBuilder(args);

    // Add services to the container.
    builder.Services.AddControllers();
    builder.Services.AddEndpointsApiExplorer();

    // Add CORS for frontend requests
    builder.Services.AddCors(options =>
    {
        options.AddPolicy("AllowFrontend",
            policy =>
            {
                policy.WithOrigins("http://localhost:3000", "http://localhost:4200", "http://localhost:5173")
                      .AllowAnyHeader()
                      .AllowAnyMethod()
                      .AllowCredentials();
            });
    });

    // Add Swagger/OpenAPI
    builder.Services.AddSwaggerGen();

    var app = builder.Build();

    // Configure the HTTP request pipeline.
    if (app.Environment.IsDevelopment())
    {
        app.UseSwagger();
        app.UseSwaggerUI();
    }

    app.UseHttpsRedirection();
    app.UseAuthorization();
    app.MapControllers().RequireCors("AllowFrontend");

    app.Run();
    ```

10. In the terminal, start your server by entering:

    ```
    dotnet run
    ```

11. In your browser, visit the Swagger URL shown in your terminal output (typically **http://localhost:5093/swagger** or similar) to see the Swagger UI and make sure your app is working

    > **Note:** Newer .NET templates use dynamic port assignment. Check your terminal output for the actual port (e.g., "Now listening on: http://localhost:5093"). The port may vary between 5093, 5000, 5001, etc.
    > 
    > **Swagger Troubleshooting:** If Swagger UI shows 404, confirm you've installed the Swashbuckle.AspNetCore package and updated Program.cs as shown above. The newer .NET templates use OpenAPI by default, which requires different setup than traditional Swagger.

12. In your text editor, create a **.gitignore** file to exclude build artifacts and sensitive files:

    ```gitignore
    # Build output
    bin/
    obj/

    # Database files
    *.db
    *.sqlite*

    # User secrets
    appsettings.Development.json

    # IDE files
    .vscode/
    .vs/
    *.swp
    *.swo

    # OS files
    .DS_Store
    Thumbs.db

    # Package restore
    project.lock.json
    project.fragment.lock.json
    artifacts/
    ```

    > **Git Best Practices:** This .gitignore file prevents committing:
    > - **Build artifacts** (`bin/`, `obj/`) that are auto-generated
    > - **Database files** (`.db`, `.sqlite`) that contain local data
    > - **IDE-specific files** that vary by developer setup
    > - **OS-specific files** like `.DS_Store` on macOS
    > 
    > For a complete .gitignore template, you can use GitHub's [VisualStudio.gitignore](https://github.com/github/gitignore/blob/main/VisualStudio.gitignore) template.

> In your terminal, open a new tab by entering the shortcut Command T, then use git to save a version of your code.
> ```bash
> git init
> git add --all
> git commit -m "Initial commit"
> ```

## API Testing Options: Swagger UI vs HTTPie

Before we build our API endpoints, let's understand the different ways to test them:

### **Swagger UI** (Built into .NET)
**What is it?** Swagger UI is a **visual, interactive documentation** tool that's automatically generated from your .NET API code. Think of it as a **web-based testing interface** for your API.

**How to use it:**
1. Start your API with `dotnet run`
2. Visit `http://localhost:5093/swagger` (or your specific port)
3. You'll see a **web page listing all your endpoints**
4. Click any endpoint to **expand it and see details**
5. Click **"Try it out"** → fill in parameters → click **"Execute"**
6. See the **response data and status codes** right in the browser

**Why use Swagger?**
- ✅ **No additional tools needed** - works in any browser
- ✅ **Visual interface** - easy to see all available endpoints
- ✅ **Auto-generated documentation** - always up-to-date with your code
- ✅ **Built-in validation** - shows required fields and data types
- ✅ **Great for sharing** - send the URL to teammates or frontend developers

### **HTTPie/curl/Postman** (Command Line & External Tools)
**What are they?** Command-line tools (HTTPie, curl) or desktop apps (Postman) for making HTTP requests.

**Example with HTTPie:**
```bash
# GET request
http GET localhost:5093/api/photos

# POST request  
http POST localhost:5093/api/photos name="test photo" url="https://via.placeholder.com/800x600" width:=800 height:=600
```

**Why use HTTPie/curl/Postman?**
- ✅ **Scriptable** - can be automated or saved in scripts
- ✅ **Familiar** - if you're already comfortable with these tools
- ✅ **Powerful** - more control over headers, authentication, etc.
- ✅ **Cross-platform** - works with any API, not just .NET

### **Recommendation**
- **New to APIs?** Start with **Swagger UI** - it's visual and beginner-friendly
- **Experienced with HTTPie?** Use **whatever you prefer** - both work great!
- **Working with a team?** **Swagger UI** is excellent for sharing and documentation

> Throughout this guide, we'll mention both options, but feel free to use whichever tool you're most comfortable with!

## Create the data layer

> **Note about namespaces:** Throughout this guide, we use "PhotoApi" as the example project name in namespaces. This corresponds to the project name "photo-api" created with `dotnet new webapi -n photo-api`. If you used a different project name, replace "PhotoApi" with your actual project name in Pascal case (e.g., "my-awesome-api" → "MyAwesomeApi").

1.  In the terminal, create a new model by opening your text editor:

    ```
    code .
    ```

2.  In the terminal, create the required directories:

    ```
    mkdir Models
    mkdir Data
    ```

3.  In your text editor, create a new file **Models/Photo.cs**:

    ```csharp
    namespace PhotoApi.Models  // Change "PhotoApi" to your actual project name
    {
        public class Photo
        {
            public int Id { get; set; }
            public string Name { get; set; } = "";
            public string Url { get; set; } = "";
            public int Width { get; set; }
            public int Height { get; set; }
            public DateTime CreatedAt { get; set; } // Set automatically in ApplicationDbContext
            public DateTime UpdatedAt { get; set; } // Set automatically in ApplicationDbContext
        }
    }
    ```

    > **Frontend Compatibility Note:** The `Url` property is included to ensure compatibility with frontend frameworks like Angular that expect a URL field for displaying images. This makes your API work seamlessly with the Angular CRUD guide's Photo interface.

4.  In your text editor, create a new file **Data/ApplicationDbContext.cs**:

    ```csharp
    using Microsoft.EntityFrameworkCore;
    using PhotoApi.Models;  // Change "PhotoApi" to your actual project name

    namespace PhotoApi.Data  // Change "PhotoApi" to your actual project name
    {
        public class ApplicationDbContext : DbContext
        {
            public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options)
            {
            }

            public DbSet<Photo> Photos { get; set; }

            public override int SaveChanges()
            {
                UpdateTimestamps();
                return base.SaveChanges();
            }

            public override async Task<int> SaveChangesAsync(CancellationToken cancellationToken = default)
            {
                UpdateTimestamps();
                return await base.SaveChangesAsync(cancellationToken);
            }

            private void UpdateTimestamps()
            {
                var entries = ChangeTracker.Entries()
                    .Where(e => e.Entity is Photo && (e.State == EntityState.Added || e.State == EntityState.Modified));

                foreach (var entityEntry in entries)
                {
                    var photo = (Photo)entityEntry.Entity;
                    
                    if (entityEntry.State == EntityState.Added)
                    {
                        photo.CreatedAt = DateTime.UtcNow;
                    }
                    
                    photo.UpdatedAt = DateTime.UtcNow;
                }
            }
        }
    }
    ```

5.  In your text editor, modify **Program.cs** to add Entity Framework (update your existing Program.cs):

    ```csharp
    using Microsoft.EntityFrameworkCore;
    using PhotoApi.Data;  // Change "PhotoApi" to your actual project name

    var builder = WebApplication.CreateBuilder(args);

    // Add services to the container.
    builder.Services.AddControllers();
    builder.Services.AddEndpointsApiExplorer();

    // Add Entity Framework
    builder.Services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(builder.Configuration.GetConnectionString("DefaultConnection") ?? "Data Source=photos.db"));

    // Add CORS for frontend requests
    builder.Services.AddCors(options =>
    {
        options.AddPolicy("AllowFrontend",
            policy =>
            {
                policy.WithOrigins("http://localhost:3000", "http://localhost:4200", "http://localhost:5173")
                      .AllowAnyHeader()
                      .AllowAnyMethod()
                      .AllowCredentials();
            });
    });

    // Add Swagger/OpenAPI
    builder.Services.AddSwaggerGen();

    var app = builder.Build();

    // Create appsettings.Development.json for database connection (development only)
    if (app.Environment.IsDevelopment())
    {
        var appSettingsDevPath = Path.Combine(Directory.GetCurrentDirectory(), "appsettings.Development.json");
        if (!File.Exists(appSettingsDevPath))
        {
            var devSettings = """
            {
              "ConnectionStrings": {
                "DefaultConnection": "Data Source=photos.db"
              },
              "Logging": {
                "LogLevel": {
                  "Default": "Information",
                  "Microsoft.AspNetCore": "Warning"
                }
              }
            }
            """;
            File.WriteAllText(appSettingsDevPath, devSettings);
        }
    }

    // Configure the HTTP request pipeline.
    if (app.Environment.IsDevelopment())
    {
        app.UseSwagger();
        app.UseSwaggerUI();
    }

    app.UseHttpsRedirection();
    app.UseCors("AllowFrontend");
    app.UseAuthorization();
    app.MapControllers();

    // Create database on startup
    using (var scope = app.Services.CreateScope())
    {
        var context = scope.ServiceProvider.GetRequiredService<ApplicationDbContext>();
        context.Database.EnsureCreated();
    }

    app.Run();
    ```

6.  In the terminal, install the Entity Framework CLI tool:

    ```
    dotnet tool install --global dotnet-ef
    ```
    
    > **Path Setup:** If the terminal shows a message about the tools directory not being on PATH, add it to your shell profile:
    > ```
    > echo 'export PATH="$PATH:/Users/$(whoami)/.dotnet/tools"' >> ~/.zshrc
    > source ~/.zshrc
    > ```
    > Then run `hash -r` to refresh the shell cache.
    > 
    > **Note:** If `dotnet-ef` still isn't found in a new terminal session, run `hash -r` to clear the shell cache.
    > 
    > **EF Tools:** Running `dotnet ef` commands will download `Microsoft.EntityFrameworkCore.Tools` automatically—no extra package needed.

7.  Create and run the database migration:

    ```
    dotnet ef migrations add InitialCreate
    dotnet ef database update
    ```

8. In your text editor, create a new file **Data/DbSeeder.cs** to seed initial data:

    ```csharp
    using System.Linq;
    using PhotoApi.Models;  // Change "PhotoApi" to your actual project name

    namespace PhotoApi.Data  // Change "PhotoApi" to your actual project name
    {
        public static class DbSeeder
        {
            public static void SeedData(ApplicationDbContext context)
            {
                if (!context.Photos.Any())
                {
                    context.Photos.AddRange(
                        new Photo { Name = "Winter", Url = "https://via.placeholder.com/200x150", Width = 200, Height = 150 },
                        new Photo { Name = "Family", Url = "https://via.placeholder.com/1024x768", Width = 1024, Height = 768 }
                    );
                    context.SaveChanges();
                }
            }
        }
    }
    ```

9. In your text editor, modify **Program.cs** to seed the database and make the Program class public for testing:

    ```diff
    // Create database on startup
    using (var scope = app.Services.CreateScope())
    {
        var context = scope.ServiceProvider.GetRequiredService<ApplicationDbContext>();
        context.Database.EnsureCreated();
    +   
    +   // Only seed data in development environment
    +   if (app.Environment.IsDevelopment())
    +   {
    +       DbSeeder.SeedData(context);
    +   }
    }

    app.Run();
    +
    + // Make Program class public for testing
    + public partial class Program { }
    ```

10. In the terminal, run the app to create and seed the database:

    ```
    dotnet run
    ```

11. You can verify the data was created by checking the **photos.db** file was created in your project directory.
    
> In your terminal, use git to save a version of your code.
> ```bash
> git add --all
> git commit -m "Add model with seeds for initial data"
> ```

## Create the web layer

> **Controller Structure:** In .NET, all your API endpoints (actions) go **inside the controller class**. We'll start with an empty controller and add one action at a time: Index → Create → Show → Update → Destroy.

1. In your text editor, create a new file **Controllers/PhotosController.cs** with the **complete structure** (including using statements):

    ```csharp
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.EntityFrameworkCore;
    using PhotoApi.Data;   // Change "PhotoApi" to your actual project name
    using PhotoApi.Models; // Change "PhotoApi" to your actual project name

    namespace PhotoApi.Controllers  // Change "PhotoApi" to your actual project name
    {
        [ApiController]
        [Route("api/[controller]")]
        public class PhotosController : ControllerBase
        {
            private readonly ApplicationDbContext _context;

            public PhotosController(ApplicationDbContext context)
            {
                _context = context;
            }
        }
    }
    ```

    > **Important:** Make sure you include **all the using statements** at the top. These are required for the controller to compile correctly.

2. Make the **INDEX** action
    - Open **Controllers/PhotosController.cs** and add the index action **inside the class, after the constructor**:
  
      ```csharp
      using Microsoft.AspNetCore.Mvc;
      using Microsoft.EntityFrameworkCore;
      using PhotoApi.Data;   // Change "PhotoApi" to your actual project name
      using PhotoApi.Models; // Change "PhotoApi" to your actual project name

      namespace PhotoApi.Controllers  // Change "PhotoApi" to your actual project name
      {
          [ApiController]
          [Route("api/[controller]")]
          public class PhotosController : ControllerBase
          {
              private readonly ApplicationDbContext _context;

              public PhotosController(ApplicationDbContext context)
              {
                  _context = context;
              }

              // ADD THE INDEX ACTION HERE:
              [HttpGet]
              public async Task<ActionResult<IEnumerable<Photo>>> Index()
              {
                  var photos = await _context.Photos.ToListAsync();
                  return Ok(photos);
              }
          }
      }
      ```
  
      > **File Structure:** All controller actions go **inside the PhotosController class**, after the constructor. We'll add each new action below the previous ones.
    - Test your API by starting the server:
      ```
      dotnet run
      ```
      
      > **Testing Your API:** You can test the INDEX endpoint using any of these methods:
      > 
      > **Method 1: Browser** (easiest for GET requests)
      > - Visit `localhost:5093/api/photos` in your browser to see the JSON output
      > 
      > **Method 2: Swagger UI** (visual and interactive)
      > - Visit `localhost:5093/swagger` in your browser
      > - Find the **GET /api/photos** endpoint and click it
      > - Click **"Try it out"** → **"Execute"** to see the response
      > 
      > **Method 3: HTTPie** (if you prefer command line)
      > ```bash
      > http GET localhost:5093/api/photos
      > ```
      > 
      > In your terminal, use git to save a version of your code.
      > ```bash
      > git add --all
      > git commit -m "Add index action"
      > ```

3. Make the **CREATE** action
    - In **Controllers/PhotosController.cs**, add the create action **below the Index action**:
  
      ```csharp
      public class PhotosController : ControllerBase
      {
          private readonly ApplicationDbContext _context;

          public PhotosController(ApplicationDbContext context)
          {
              _context = context;
          }

          [HttpGet]
          public async Task<ActionResult<IEnumerable<Photo>>> Index()
          {
              var photos = await _context.Photos.ToListAsync();
              return Ok(photos);
          }

          // ADD THE CREATE ACTION HERE (below Index):
          [HttpPost]
          public async Task<ActionResult<Photo>> Create([FromBody] CreatePhotoRequest request)
          {
              var photo = new Photo
              {
                  Name = request.Name,
                  Url = request.Url,
                  Width = request.Width,
                  Height = request.Height
              };

              _context.Photos.Add(photo);
              await _context.SaveChangesAsync();

              return Ok(photo); // Will change to CreatedAtAction once we add the Show action
          }

          // ADD THE REQUEST CLASS INSIDE THE CONTROLLER CLASS (at the bottom):
          public class CreatePhotoRequest
          {
              public string Name { get; set; } = "";
              public string Url { get; set; } = "";
              public int Width { get; set; }
              public int Height { get; set; }
              // Note: CreatedAt/UpdatedAt are set automatically, so don't include them in request bodies
          }
      }
      ```
  
      > **Where to add it:** Place the Create action **after the Index action** and **before the closing brace** of the PhotosController class. The request classes go at the bottom of the controller class.
      > 
      > **Note:** We temporarily use `Ok(photo)` instead of `CreatedAtAction(nameof(Show), ...)` because we haven't created the Show action yet. We'll fix this in the next step.
    - Test the create action:
      ```
      dotnet run
      ```
      > **Testing CREATE:** You can test the CREATE endpoint using these methods:
      > 
      > **Method 1: Swagger UI** (recommended for beginners)
      > - Visit `localhost:5093/swagger` in your browser
      > - Find the **POST /api/photos** endpoint and click it
      > - Click **"Try it out"**
      > - Fill in the JSON body with something like:
      >   ```json
      >   {
      >     "name": "test photo",
      >     "url": "https://via.placeholder.com/800x600",
      >     "width": 800,
      >     "height": 600
      >   }
      >   ```
      > - Click **"Execute"** to create the photo
      > 
      > **Method 2: HTTPie** (if you prefer command line)
      > ```bash
      > http POST localhost:5093/api/photos name="test photo" url="https://via.placeholder.com/800x600" width:=800 height:=600
      > ```
      > 
      > **Method 3: curl** (alternative command line)
      > ```bash
      > curl -X POST localhost:5093/api/photos \
      >      -H "Content-Type: application/json" \
      >      -d '{"name":"test photo","url":"https://via.placeholder.com/800x600","width":800,"height":600}'
      > ```
      > 
      > **Note:** You CANNOT test POST requests in the browser address bar (browsers default to GET requests)
      > 
      > In your terminal, use git to save a version of your code.
      > ```bash
      > git add --all
      > git commit -m "Add create action"
      > ```

4. Make the **SHOW** action
    - In **Controllers/PhotosController.cs**, add the show action **below the Create action**:
  
      ```csharp
          [HttpPost]
          public async Task<ActionResult<Photo>> Create([FromBody] CreatePhotoRequest request)
          {
              // ... Create action code ...
          }

          // ADD THE SHOW ACTION HERE (below Create):
          [HttpGet("{id}")]
          public async Task<ActionResult<Photo>> Show(int id)
          {
              var photo = await _context.Photos.FindAsync(id);
              
              if (photo == null)
              {
                  return NotFound();
              }

              return Ok(photo);
          }
      ```
  
      > **Where to add it:** Place the Show action **after the Create action**, still inside the PhotosController class.
    - **Fix the Create action:** Now that we have a Show action, update the Create action to use the proper response:
      
      ```csharp
      // In the Create action, change this line:
      return Ok(photo); // Will change to CreatedAtAction once we add the Show action
      
      // To this:
      return CreatedAtAction(nameof(Show), new { id = photo.Id }, photo);
      ```
    - Test the show action:
      ```
      dotnet run
      ```
      > **Testing SHOW:** You can test the SHOW endpoint using these methods:
      > 
      > **Method 1: Browser** (easiest)
      > - Visit `localhost:5093/api/photos/1` in your browser to see photo with ID 1
      > 
      > **Method 2: Swagger UI** (visual)
      > - Visit `localhost:5093/swagger` in your browser
      > - Find the **GET /api/photos/{id}** endpoint and click it
      > - Click **"Try it out"** → enter `1` for the ID → **"Execute"**
      > 
      > **Method 3: HTTPie** (command line)
      > ```bash
      > http GET localhost:5093/api/photos/1
      > ```
      > 
      > In your terminal, use git to save a version of your code.
      > ```bash
      > git add --all
      > git commit -m "Add show action"
      > ```

5. Make the **UPDATE** action
    - In **Controllers/PhotosController.cs**, add the update action **below the Show action**:
  
      ```csharp
          [HttpGet("{id}")]
          public async Task<ActionResult<Photo>> Show(int id)
          {
              // ... Show action code ...
          }

          // ADD THE UPDATE ACTION HERE (below Show):
          [HttpPut("{id}")]
          public async Task<ActionResult<Photo>> Update(int id, [FromBody] UpdatePhotoRequest request)
          {
              var photo = await _context.Photos.FindAsync(id);
              
              if (photo == null)
              {
                  return NotFound();
              }

              // We use PUT here to replace the full resource; switch to PATCH if you teach JSON-Patch later
              photo.Name = request.Name ?? photo.Name;
              photo.Url = request.Url ?? photo.Url;
              photo.Width = request.Width ?? photo.Width;
              photo.Height = request.Height ?? photo.Height;

              await _context.SaveChangesAsync();
              
              return Ok(photo);
          }

          // ADD THIS REQUEST CLASS at the bottom with the other request classes:
          public class UpdatePhotoRequest
          {
              public string Name { get; set; } = "";
              public string Url { get; set; } = "";
              public int? Width { get; set; }
              public int? Height { get; set; }
          }
      ```
  
      > **Where to add it:** Place the Update action **after the Show action**. Add the UpdatePhotoRequest class **at the bottom** of the controller with the other request classes.
    - Test the update action:
      ```
      dotnet run
      ```
      > **Testing UPDATE:** You can test the UPDATE endpoint using these methods:
      > 
      > **Method 1: Swagger UI** (recommended)
      > - Visit `localhost:5093/swagger` in your browser
      > - Find the **PUT /api/photos/{id}** endpoint and click it
      > - Click **"Try it out"** → enter `1` for the ID
      > - Fill in the JSON body with updates:
      >   ```json
      >   {
      >     "name": "Updated Photo Name",
      >     "url": "https://via.placeholder.com/1000x750",
      >     "width": 1000,
      >     "height": 750
      >   }
      >   ```
      > - Click **"Execute"** to update the photo
      > 
      > **Method 2: HTTPie** (command line)
      > ```bash
      > http PUT localhost:5093/api/photos/1 name="Updated Photo Name" url="https://via.placeholder.com/1000x750" width:=1000 height:=750
      > ```
      > 
      > **Method 3: curl** (alternative)
      > ```bash
      > curl -X PUT localhost:5093/api/photos/1 \
      >      -H "Content-Type: application/json" \
      >      -d '{"name":"Updated Photo Name","width":1000,"height":750}'
      > ```
      > 
      > **Note:** You CANNOT test PUT requests in the browser address bar
      > 
      > In your terminal, use git to save a version of your code.
      > ```bash
      > git add --all
      > git commit -m "Add update action"
      > ```

6. Make the **DESTROY** action
    - In **Controllers/PhotosController.cs**, add the destroy action **below the Update action**:
  
      ```csharp
          [HttpPut("{id}")]
          public async Task<ActionResult<Photo>> Update(int id, [FromBody] UpdatePhotoRequest request)
          {
              // ... Update action code ...
          }

          // ADD THE DESTROY ACTION HERE (below Update):
          [HttpDelete("{id}")]
          public async Task<ActionResult> Destroy(int id)
          {
              var photo = await _context.Photos.FindAsync(id);
              
              if (photo == null)
              {
                  return NotFound();
              }

              _context.Photos.Remove(photo);
              await _context.SaveChangesAsync();
              
              return NoContent(); // HTTP 204 - successful deletion with no content
          }

          // Request classes go here at the bottom...
      }
      ```
  
      > **Where to add it:** Place the Destroy action **after the Update action** and **before the request classes** at the bottom of the controller class.
    - Test the destroy action:
      ```
      dotnet run
      ```
      > **Testing DESTROY:** You can test the DESTROY endpoint using these methods:
      > 
      > **Method 1: Swagger UI** (recommended)
      > - Visit `localhost:5093/swagger` in your browser
      > - Find the **DELETE /api/photos/{id}** endpoint and click it
      > - Click **"Try it out"** → enter `3` for the ID (or any existing photo ID)
      > - Click **"Execute"** to delete the photo
      > - You should get a **204 No Content** response (successful deletion)
      > 
      > **Method 2: HTTPie** (command line)
      > ```bash
      > http DELETE localhost:5093/api/photos/3
      > ```
      > 
      > **Method 3: curl** (alternative)
      > ```bash
      > curl -X DELETE localhost:5093/api/photos/3
      > ```
      > 
      > **Verify deletion:** Use the INDEX endpoint to confirm the photo was deleted:
      > ```bash
      > http GET localhost:5093/api/photos    # or visit localhost:5093/api/photos in browser
      > ```
      > 
      > **Note:** You CANNOT test DELETE requests in the browser address bar
      > 
      > In your terminal, use git to save a version of your code.
      > ```bash
      > git add --all
      > git commit -m "Add destroy action"
      > ```

## Final project structure

Your final **Controllers/PhotosController.cs** should have this structure (**all actions go inside the PhotosController class**):

```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using PhotoApi.Data;   // Change "PhotoApi" to your actual project name
using PhotoApi.Models; // Change "PhotoApi" to your actual project name

namespace PhotoApi.Controllers  // Change "PhotoApi" to your actual project name
{
    [ApiController]
    [Route("api/[controller]")]
    public class PhotosController : ControllerBase
    {
        private readonly ApplicationDbContext _context;

        public PhotosController(ApplicationDbContext context)
        {
            _context = context;
        }

        [HttpGet]
        public async Task<ActionResult<IEnumerable<Photo>>> Index()
        {
            var photos = await _context.Photos.ToListAsync();
            return Ok(photos);
        }

        [HttpPost]
        public async Task<ActionResult<Photo>> Create([FromBody] CreatePhotoRequest request)
        {
            var photo = new Photo
            {
                Name = request.Name,
                Url = request.Url,
                Width = request.Width,
                Height = request.Height
            };

            _context.Photos.Add(photo);
            await _context.SaveChangesAsync();

            return CreatedAtAction(nameof(Show), new { id = photo.Id }, photo);
        }

        [HttpGet("{id}")]
        public async Task<ActionResult<Photo>> Show(int id)
        {
            var photo = await _context.Photos.FindAsync(id);
            
            if (photo == null)
            {
                return NotFound();
            }

            return Ok(photo);
        }

        [HttpPut("{id}")]
        public async Task<ActionResult<Photo>> Update(int id, [FromBody] UpdatePhotoRequest request)
        {
            var photo = await _context.Photos.FindAsync(id);
            
            if (photo == null)
            {
                return NotFound();
            }

            // We use PUT here to replace the full resource; switch to PATCH if you teach JSON-Patch later
            photo.Name = request.Name ?? photo.Name;
            photo.Url = request.Url ?? photo.Url;
            photo.Width = request.Width ?? photo.Width;
            photo.Height = request.Height ?? photo.Height;

            await _context.SaveChangesAsync();
            
            return Ok(photo);
        }

        [HttpDelete("{id}")]
        public async Task<ActionResult> Destroy(int id)
        {
            var photo = await _context.Photos.FindAsync(id);
            
            if (photo == null)
            {
                return NotFound();
            }

            _context.Photos.Remove(photo);
            await _context.SaveChangesAsync();
            
            return NoContent(); // HTTP 204 - successful deletion with no content
        }

        public class CreatePhotoRequest
        {
            public string Name { get; set; } = "";
            public string Url { get; set; } = "";
            public int Width { get; set; }
            public int Height { get; set; }
        }

        public class UpdatePhotoRequest
        {
            public string Name { get; set; } = "";
            public string Url { get; set; } = "";
            public int? Width { get; set; }
            public int? Height { get; set; }
        }
    }
}
```

## Testing your API

Run your server:
```
dotnet run
```

Your API will be available at (replace 5093 with your actual port):
- **Index (GET)**: `http://localhost:5093/api/photos`
- **Create (POST)**: `http://localhost:5093/api/photos`
- **Show (GET)**: `http://localhost:5093/api/photos/{id}`
- **Update (PUT)**: `http://localhost:5093/api/photos/{id}`
- **Destroy (DELETE)**: `http://localhost:5093/api/photos/{id}`

### **Testing Options**

**Option 1: Swagger UI** (Visual & Interactive)
- Visit `http://localhost:5093/swagger` in your browser
- See all endpoints, try them out, and view responses
- Perfect for exploration and sharing with teammates

**Option 2: HTTPie** (Command Line)
```bash
# Test all endpoints
http GET localhost:5093/api/photos
http POST localhost:5093/api/photos name="test" url="https://via.placeholder.com/800x600" width:=800 height:=600
http GET localhost:5093/api/photos/1
http PUT localhost:5093/api/photos/1 name="updated" url="https://via.placeholder.com/400x300"
http DELETE localhost:5093/api/photos/1
```

**Option 3: Browser** (GET requests only)
- Visit URLs directly for GET requests: `localhost:5093/api/photos`

Choose whichever method you prefer - they all work with the same API!

## Additional Configuration

### Database Migrations in Version Control

> **Migrations Note:** We recommend committing the generated `Migrations/` folder so everyone has the same database schema. If you prefer regenerating migrations locally, add `Migrations/` to your .gitignore file.

### Build Error Troubleshooting

If you get compilation errors like "The type or namespace name 'ControllerBase' could not be found", make sure your **Controllers/PhotosController.cs** file includes all the necessary using statements at the top:

```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using PhotoApi.Data;
using PhotoApi.Models;

namespace PhotoApi.Controllers
{
    // ... rest of your controller code
}
```

### CORS Troubleshooting

If your frontend runs on a different port, update the CORS configuration in **Program.cs**:
```csharp
policy.WithOrigins("http://localhost:3000", "http://localhost:4200", "http://localhost:5173", "http://localhost:8080")
```
Add your specific frontend dev server port to this list.

> **CORS Note:** We use global `app.UseCors()` which applies the CORS policy to all endpoints. Make sure to place `app.UseCors()` before `app.UseAuthorization()` in the middleware pipeline.

## Summary

Congratulations! You've built a complete .NET Web API with:
- **Entity Framework Core** for data access
- **SQLite** database with automatic migrations
- **Full CRUD operations** (Create, Read, Update, Delete)
- **Swagger/OpenAPI** documentation for interactive testing
- **CORS configuration** for frontend integration
- **Manual testing** using browser, Swagger UI, and REST tools

This API provides the same REST endpoints as our Rails API guide, demonstrating how the same concepts (HTTP verbs, status codes, JSON responses) work across different technology stacks. Your frontend applications can now connect to either Rails or .NET backends using identical HTTP requests!

> Final git commit:
> ```bash
> git add --all
> git commit -m "Complete CRUD API with all endpoints"
> ``` 