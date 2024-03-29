# Module 4: Create a Form 

*Module goal: 
- *Learn how to create a Form with ASP.NET Core*
- *Overview of the to ASP.NET Core Web Application template*
- *Introduction to [Web API](https://docs.asp.net/en/latest/tutorials/first-web-api.html)*

## Create a new Web Application
*Option 1: Open new project with Visual Studio*
- Create a new ASP.NET Core App by go to File New Project ->.NETCore -> ASP.NET Core Web Application (.NET Core) and named it as `MusicStore` 

 ![](module4-gif/create-web-app-mvc.gif)

*Option 2: Open new project with .Net CLI*
- Again, create a directory named "MyWebApp" and in the folder use the command below to create a Web App with command line.
  
    ```
    mkdir MusicStore
    cd MusicStore
    dotnet new mvc
    code .
    ```
> **Note:** This module require you to create project with Visual Studio, *Option 2* is just an additional information

### Create a Model 
- We need to make a model for Album in order to create it
- Right click on the Model folder --> Add --> Class --> Albums
```C#
  public class Albums
    {
        public int id { get; set; }
        public String name { get; set; }
        public String artist { get; set; }
        public String genre { get; set; }
        public DateTime release_date { get; set; }
        public decimal price { get; set; }
    }
```
![](module4-gif/create-album-model.gif)

### Create a controller 
- Right click on the Controller folder --> Add --> scaffolded items 
- Select the MVC Controller with Views, using Entity Framework 

![image](https://cloud.githubusercontent.com/assets/2546640/23244833/09e91150-f956-11e6-9c4b-ddca1961fe65.PNG)

- Select your Model Class : albums(MusicStore.Models)

![image](https://cloud.githubusercontent.com/assets/2546640/23244907/80064204-f956-11e6-8b25-1b84e6227c98.PNG)

- Select Data Context class: ApplicationDbContext(MusicStore.Data)

![image](https://cloud.githubusercontent.com/assets/2546640/23244967/ea4259a0-f956-11e6-83f2-5457b0cd248a.PNG)

- Now, you should that this has scaffolded a new AlbumsControllers and 5 new Views

*Albums Controller*

![image](https://cloud.githubusercontent.com/assets/2546640/23245055/b878ec6c-f957-11e6-889a-372ea2e4bdf3.PNG)

*Albums Views*

![image](https://cloud.githubusercontent.com/assets/2546640/23245066/bff368c8-f957-11e6-97d7-c5fead3e9b78.PNG)

- Run the application and navigate to /albums

*Should see the errror below*

![image](https://cloud.githubusercontent.com/assets/2546640/23279240/72048792-f9e2-11e6-89de-0e02b16e1a78.PNG)

**Add Database**

*Option 1: Use Visual Studio*

In Visual Studio, use the Package Manager Console to scaffold a new migration for these changes and apply them to the database:

>PM> Add-Migration [migration name]

>PM> Update-Database 

*Option 2: Use dotnet CLI*
> dotnet ef migrations add [migration name] 

> dotnet ef database update 

- Run your application again and go to /albums

![image](https://cloud.githubusercontent.com/assets/2546640/23284042/4e6c78de-f9f7-11e6-97ed-67e2fc0b6431.PNG)

 - Click Create new

 *Quick note: This was an HTTP GET* 

 ![image](https://cloud.githubusercontent.com/assets/2546640/23285222/d7667f26-f9fd-11e6-878d-84278f40ecca.PNG)

- Add an entry an album and hit create
![image](https://cloud.githubusercontent.com/assets/2546640/23369507/8bc3258a-fcdf-11e6-913c-7ece4270deae.PNG)
*Quick note: By simply creating a new item you have performed a Post Redirect Get [(PRG)](https://en.wikipedia.org/wiki/Post/Redirect/Get).*

******
## Form Validation 

- Open your albums Model (albums.cs) 
```C#
namespace MusicStore.Models
{
    public class albums
    {
        public int ID { get; set; }
        public String Artist { get; set; }
        public String Album { get; set; }
        public String Genre { get; set; }
        public DateTime ReleaseDate { get; set; }
        public decimal Price { get; set; }
    }
}
```
- Let's add the following validation to the form ([Learn more here](https://docs.microsoft.com/en-us/aspnet/core/mvc/models/validation))

    - Album title needs to be at least 3 characters
    - And the Genre shouldn't have crazy characters
    - Albums Price should be under $20 
- Add `using System.ComponentModel.DataAnnotations;`
```C#
namespace MusicStore.Models
{
    public class albums
    {
        public int ID { get; set; }

        [Required]
        public String Artist { get; set; }

        [StringLength(60, MinimumLength =3)]
        [Required]
        public String Album { get; set; }

        [RegularExpression(@"^[A-Z]+[a-zA-Z''-'\s]*$")]
        public String Genre { get; set; }

        [Required]
        public DateTime ReleaseDate { get; set; }

        [Required]
        [Range(0, 20.00)]
        public decimal Price { get; set; }
    }
}

```
- Run the application and navigate to albums/Create 

*Test validation by entering 1 for Album and $ for Genre*

![image](https://cloud.githubusercontent.com/assets/2546640/23374172/130540c6-fcf1-11e6-9b39-8cb9ec63c931.PNG)

