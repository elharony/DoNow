# DoNow - The simpest To-Do List App!
DoNow is a Web Application built to save your day-to-day tasks in a simpler way, it saves your data (When opens on the same browser) without signing up. Very fast, and functional app!

## Getting Started
- `git clone https://github.com/elharony/DoNow.git`
- `cd DoNow`
- `npm install`
- `start index.html`

## How to run the tests?
- `cd test/`
- `start SpecRunner.html`

## Docs
The project codebase relies on `MVC`. Which stands for: **Model**, **View** and **Controller**, and it's one of the most common Software Design Pattern to build scalable and maintainable systems. It consists of 3 parts:
- **Model:** Manages the application data. Where we have `create`, `read`, `update` and `delete` methods (CRUD Operations) of our ToDo!
- **View:** Visualize the model (i.e. How we display the data). Where we render HTML in the DOM, and the most common method is `render`!
- **Controller:** Manages the relation between Model & View.

Let's get into the following examples to know how it really works:

**1) Example:**
When the user Adds New ToDo
- The `Controller` has a method called `addItem` which takes the user input
- First of all, it make sure that input is a valid input (Doesn't contain any unacceptable characters)
- Then, it calls the `create` method from the `Model` (To create that new item to our database)
- After that, there's a callback function to the `View` to re-render our ToDo List, and update the filter!

**2) Example:**
When the user Edits Existing ToDo
- The `Controller` has a method called `editItem` which takes the selected item ID
- In the `Model`, there's a method called `read`, and it takes 2 parameters (Item ID, Callback Func)
- We will pass the `id`, and the `callback` function
- That `callback` will affect the `View` by passing the `editItem` as a **View Command** to the `render` method
- Now, the whole `View` will re-render, and the **Item Editing Mode** will be activated

**Conclusion:**
- **Model:** Is where all data-related functions are.
- **View:** Contains a list of separated `View Commands`, and each command manipulates the DOM to visualize the `data` we received from the `Model`.
- **Controller:** Is where all the magic happens. It represents the Brain of our Application, it determines **when** to call **what**. Basically, when a `Controller` method triggered, it handles the user inputs, calls a specific `Model` method to do any CRUD Operation, with the right `callback` function to update the `View` for the user!

This is a very good illustration to explain the `MVC` concept:
![MVC-Design-Pattern](https://user-images.githubusercontent.com/16986422/69777269-2f105980-11a8-11ea-867b-72d0aa60e348.png)

- First of all, the `User` opens our application, write a new todo, and pressed **Enter** to add it. That action triggers the `Controller`
- The `Controllers` has the user input, can validate it, and pass it to the `Model`
- At this point, the `Model` will do 2 things:
    - Add that new todo to our database
    - Triggers the `View` with a specific **View Command** _(i.e.removeItem, setFilter)_
- The `View` will re-render, update the filter according to the View Command it receives from the `Model`
- Now, the `User` will see the new todo on the **All** list!!

## Performance Analysis
In this part, we are going to discuss DoNow Performance against [Todolistme](http://todolistme.net/) Performance. Here's the final result:

| Factor | DoNow | [Todolistme](http://todolistme.net/) |
| ------ | ----- | ------------------------------------ |
| Performance | 98 | 61 |
| Best Practices | 93 | 71 |
| Accessibility | 60 | 38 |
| SEO | 75 | 78 |

Let's get into each website's performance report:

<details>
  <summary>Todolistme Performance</summary>
  
According to Google Lighthouse Performance Audit tool, our competitor site has the following key performance numbers:
- Performance: 88
- Best Practices: 71
- Accessibility: 38
- SEO: 78

> We will only covers the `Performance` and `Best Practices` factors.

#### 1) Performance
There are so many problems affect the website performance, here are some of them:
- Using too many blocking-external JS files. For example; They are using [Twitter Widgets](http://platform.twitter.com/widgets.js), [Google Analytics](https://www.google-analytics.com/ga.js), Google Ads ([1](http://pagead2.googlesyndication.com/pagead/show_ads.js), [2](http://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js)), [Facebook Integration](http://connect.facebook.net/en_US/all.js#xfbml=1), and more!

**Solution:** Although most of them are essentials at some point in the application, we can use `async` and `defer` approaches. Basically, we will tell JS to wait, until the rest of the page loads, then those "non-critical" JS files will be loaded `Asynchronously`!

- Don't specify an expiration date for the files.

**Solution:** Set a Cache Expiration date for all external resources. We can achieve it using `.htaccess` or pre-built plugins if we are using a CMS (i.e. WordPress)!

- Minify the JS files. Minifying means deleting all whitespaces/lines, and condense all the code. It's one of the most common "bad practices" to use unmified files on production!

**Solution:** We export a minified JS files for the production build of the website, and we always leave the unmified version for developers to maintain the code. It's very common to use any Task Runner (i.e. [Webpack](https://webpack.js.org/), [Gulp](https://gulpjs.com/), [Grunt](https://gruntjs.com/) to do this task!

- There are 20 requests to get very small images. Total website requests are 64 requests (According to [GTMetrix](https://gtmetrix.com/reports/todolistme.net/KPFKwECz)), so it's 1/3 of the total website requests, and once fixed, it will dramatically improve the overall performance.

**Solution:** There is a technique called: `Combine images using CSS sprites`. We use it to combine tiny/small images into one single image and use the `background-position` CSS Property to get the exact icon/tiny image we need for each element. Now, we only 1 request, not 20. This technique is applied by - almost - all big companies. For example; Amazon wraps all Localization/Language switcher & Country Flags in one image, [check it out!](https://m.media-amazon.com/images/G/01/AUIClients/InternationalCustomerPreferencesNavAssets-icp_sprite-0b528ccc99b2eed18447291de6df851bc2c6fe68._V2_.png)

#### 2) Best Practices
Although `Best Practices` don't affect the Website Loading Time as the `Performance` issues do, but it really affects the overall User Experience... Here are some problems, and possible ways to solve:
- The competitor site doesn't have an SSL Certificate installed. Basically, all requests are sending via `HTTP` not `HTTPS` which isn't secured, and the users will notice the "Not Secure" label whenever they visit the website.

**Solution:** It needs a valid SSL Certificate, which could be either purchased via the Hosting Provider/3rd-party Website or get one for free using LetsEncrypt [I wrote an article about it a while ago, check it out & install SSL Certificate in 5 minutes!](https://www.elharony.com/get-your-free-ssl-certificate-with-lets-encrypt/)

- It doesn't use `HTTPS` to get external resources. It looks like the same thing, right? Not really. Many websites have SSL Certificate Installed, but they still use `HTTP` resources!

**Solution:** After installing SSL Certificate on the website, make sure that ALL external sources (i.e. Images, CSS, JS) are using `HTTPS` not `HTTP`

- This website is using a Vulnerable Version of jQuery (jQuery@2.2.4). Which isn't secure anymore to use!

**Solution:** There are 2 possible solutions here; Get rid of jQuery, and convert the codebase to Vanilla JavaScript (Which can do pretty much everything now!). Or update the jQuery version to a recent one!

</details>


<details>
  <summary>DoNow Performance</summary>
  
According to Google Lighthouse Performance Audit tool, DoNow has the following key performance numbers:
- Performance: 98
- Best Practices: 93
- Accessibility: 60
- SEO: 75

> We will only covers the `Performance` and `Best Practices` factors.

#### 1) Performance
Our performance is PERFECT. We almost don't need to do anything here!

#### 2) Best Practices
It's score is `86` but with a little fix, it will be `93`. All you need to do is to comment `node_modules/todomvc-common/base.js:245` as follows:
```javascript
// getFile('learn.json', Learn);
```

Also, we are facing a common issue `Does not use HTTP/2 for all of its resources` with 11 requests. It's also exist on our competitor site, with more details above!

</details>





## Contributing
This is my 8th Project at [OpenClassrooms Front-End Developer Diploma](https://openclassrooms.com/en/paths/61-front-end-developer), and I won't maintain this project. But if you're interested in this project, your can `Fork` it, and build something amazing on top of it!