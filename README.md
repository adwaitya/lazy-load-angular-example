# lazy-load-angular-example


Lazy Loading Angular Modules
In this article we’re going to see how to lazy-load Angular modules, a feature that could help you a lot with large and heavy applications. By lazy-loading a module we can tell Angular to load it only when its route is been visited, meaning that its weight won’t affect the size of your application at startup.

Set-up
We are going to initialize the application with the Angular CLI, which is an extremely useful tool for speeding up our development process. If you haven’t installed the CLI, make sure you installed Node 6.9.0 or higher and NPM 3 or higher, then run this command:

npm install -g @angular/cli
Then, initialize an empty application:

ng new lazy-test --routing
With this command we are telling the CLI to create for us an empty project with an additional routing module already set up for us. Wonderful! This is what it looks like:


app-routing.module.ts
Creating the components
Let’s create our first component, which will be loaded normally.

cd lazy-test
ng generate component first
This will generate a first folder which will contain all of our new component’s files, and the FirstComponent will be automatically imported in our AppModule. Let’s go into app-routing.module.ts and add this component to the main route:


app-routing.module.ts
We are using pathMatch: ‘full’ because our route is an empty one, and we don’t want it to match any other route. In order to clear some stuff, go to app.component.html and delete everything but <router-outlet></router-outlet>, which is where our components will be displayed by the router.
Now, let’s use the CLI again to run the application in our browser:

ng serve --open
We should successfully see the paragraph “first works!” in our browser: beautiful, our component is displayed correctly by the router!

Creating a lazy-loaded module
Now we can create an additional module for this app, which will be lazy-loaded. Let’s use the CLI to achieve this, with this command:

ng generate module lazy
We also want to create a component which will be the root component for our new lazy-loaded module: we just tell the CLI the path to our module and it will create our files for us, importing them in the correct module:

ng generate component lazy/second
We just need to declare a new set of routes for our lazy module, which will display our SecondComponent.


lazy.module.ts
Here, we’re using an empty path because these will be the relative routes for this module, not for the entire application. Also, for the same reason, we use RouterModule.forChild() instead of forRoot().

Lazy-loading the new module
Now we need to go to app-routing.module.ts and create another route for our new LazyModule: we won’t specify any component! Instead, we’ll specify the path to the module, followed by the name of the module’s class with a hashtag. Also, we’ll use the special keyword loadChildren:


app-routing.module.ts
Don’t import LazyModule for any reason! If you do that, you’ll lose all the advantages of lazy-loading and you may have serious problems with debugging: specifying the module’s path is all we need to do to associate it with our route, let Angular do the rest.
A simple navbar
In order to see our new routing system in action, let’s go to app.component.html and modify it slightly:


app.component.html
This way we can test our application without the need to reload the browser window! Now we can navigate from AppModule to LazyModule with our primitive navbar by clicking on the relative links: you’ll see that the browser path is changed and the lazy module is loaded correctly.

For truth’s sake, we can open the browser inspector and go to the Network tab: you’ll see that once you navigate to the lazy route, a new chunk.js file will be loaded asynchronously: that’s our LazyModule in action.