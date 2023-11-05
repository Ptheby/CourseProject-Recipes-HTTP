# CourseProject-Recipes-HTTP
 
1. Module Intro
2. Lecture 2 - Backend/Firebase Setup
    -create new project and i am naming in "ng-course-recipe-book" then a realtime database where we are given the URL we use to post/fetch data.
3. Lecure 3- SEttung up the Data Storage Service
   -Create a new service called data-storage and i do this automaticaly with ng g s data-storage.
   ///we can also do this in the recipe service but am doing it this way to keep everyhing cleaner and easier////
   -Import the HttpClientModule in the apps.module.ts @angular/common/http and also import that in the new data-storage service where we have also imported Inectable and we will inject the http abilities in this this new service at the constructor.
    =======Entire new Service so Far======
   import { HttpClientModule } from '@angular/common/http';
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class DataStorageService {

  constructor(private http: HttpClientModule)) { }
}
=================================================
Video 4-  Storing Recipes 
First we import the RecipeService into the new data storage service ts and then inject it by adding into the parameters of the constructor as well like  private recipeService: RecipeService and then we create a new method called storeRecipes where
we pass no arguments and in the bosy we create a new varibale in here calls recipes which is equal to this.recipeService.getRecipes();   where we have access to the recipe service and its method getRecipes to get our recipes and then this is assinging it to the new variable recipes that i just mentioned. THEN, we output this by invoking the http as well so the next line is this.http.put('firebaseURLwemae, then recipes as the data we are passing to the url and then immediately we subscribe to this otherwise it will not get broadcasts and in the suscbribe method we lable response callback conscole log response. 

data-storage-ts
====================================================
import { HttpClientModule } from '@angular/common/http';
import { Injectable } from '@angular/core';
import { RecipeService } from '../recipes/recipe.service';


@Injectable({
  providedIn: 'root'
})
export class DataStorageService {

  constructor(private http: HttpClientModule, private recipeService: RecipeService) { }


storeRecipes() {
  const recipes = this.recipeService.getRecipes();
  this.http.put('https://ng-course-recipe-book-d1c89-default-rtdb.firebaseio.com/', recipes)
  .subscribe(response => {
    console.log(response)
  });
}

}
===================================
then we have to make sure that new storeRecipes() is getting called and we do that in the header compoent HTML by adding a click listener tied to again a new method we name "onSaveData(), where then  we define that in the header component TS by simply naming onSaveData() {
 then running the new method on the click listneer like this ::: this.dataStorageService.storeRecipes();
by 
Header HTML
=============
   <li><a style="cursor: pointer;" (click)="onSaveData()">Save Data</a></li>

   Header TS
   ============
export class HeaderComponent {
  constructor(private dataStorageService: DataStorageService) {}

  onSaveData() {
    this.dataStorageService.storeRecipes();
  }
}

My code is broken somewhere so calling in to help!  called in and zelda helped me!

Video 5 Fetching Recipes
-here we can go back to the new data storage service we made where we will first create a new method caled "fetchRecipes" where we will call on the http url get with a angled bracket of <Recipe[] > so that Angular knows our response will resemb;e our recipes array model which we now also have to import. and it pulls data from our firebase URL/recipes.json, 

Datastorage service
================
  fetchRecipes() {
    this.http.get<Recipe[]>('https://ng-course-recipe-book-d1c89-default-rtdb.firebaseio.com/recipes.json')
    .subscribe(recipes => {
      this.recipeService.setRecipes(recipes);
    })
  }

and we also subscribe right after that and pass recipes => amd  this.recipeService.setRecipes(recipe) which we created in the recipe service. 
  Recipe Service
  ============
 setRecipes(recipes:Recipe[]) {
    this.recipes= recipes;
    this.recipesChanged.next(this.recipes.slice())
  }




and we also subscribe right after that and pass recipes => amd  this.recipeService.setRecipes(recipe) which we created in the recipe service. 









