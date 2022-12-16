# GameStep
=======


GameStep is a small JavaScript file that fetches Markdown files and renders them
as full pages. Essentially, it's the easiest
way to make open source documentation from *Readme* files.

GameStep SDK functions as following:
1. Retreiving steps data from Apple HealthKit.
2. Converting steps to coins (100 steps = 1 coin).
3. Store the coins and steps data permanantly to the phone through Apple Developer Tool ```UserDefault``` class.
4. Open up a Coins interface for developer to manipulate the SDK.
### Retreiving steps data from Apple HealthKit.

The original code for accessing the healthkit data is from azarmshap on youtube, https://www.youtube.com/watch?v=ohgrzM9gfvM And it was revised to match the data mapping.

The folder ```HealthInfo``` is where the SDK get access the data. The developer need to manually add the Healthkit capability from XCode and sign fo the privacy info list for accessing user's health data.

The file HealthStore contains the major methods like: asking for authorization from the user to get access to healthkit, fetching the steps data in a given date range, and get the today's step count so far whenever the user launches the app. In the current file, the query in the function ```fetchStep``` will fetch the data of today in a 1 hr frequency, which can be further extend to 1 week, 1 month and further, and the frequency can be narrowed to 1 min or 1 sec if necessary. In future versions, we will open up an interface for developer to customize the time and frequency manner when accessing the steps data. It will return the a Healthkit collection which will be further be transformed into a collection of array of Step object, which initialize with ```date```, ```count``` and ```UUID```. The function ```getStep``` will directly return a double that contains today's step so far whenever this function gets called.

The step is used to represent a customized data period of steps. It has attributes like ```step``` ```count```, ```UUID``` and ```date```.

### Converting steps to coins (100 steps = 1 coin).

The ```GameStep``` file act as a main controller of the SDK, which it receive the steps data from the healthkit access, handle calls from the developer, and do the converting and storing procedure. 

The ```GameStep``` main class have variables like:
- ```healthstore``` is used to retreive steps from healthkit.
- ```currentStep```, is used to record the newest step retreived from the healthkit.
- ```theCoin``` is used to record the newest coin converted.


### Store the data
```defaults``` stores the the last state of steps and coins from last update of before the user quit the app from last time.
***

## How to use GameStep SDK
1. Import the package.
2. Initialize the instance of GameStep.
3. Use the interface for the coins and steps to manipulate.
  - ```getCoins``` will return the current coin the user have.
      - The ```getCoins``` will only be recalculated when user relaunch the app. During a single session, the coin would not update itself since the step is not updated.
  - ```consumeCoins``` will consume the coin for game modifying.
  - Step is also opened to user through ```getSteps``` if the user needed.
      - To better motivate the user to exercise, the ```getStep``` method will get the step data from today's step data from 12am. As a result of that, the all step data will recount for each day, and to encourage the user to use the app, the user have to launch the app to convert the coin to store it permamently.
5. The SDK will handle the update and store new data whenever the function were called.

In future versions, if applicable, we hope the SDK will explore more powerful features in the SDK and build a platform for it. 

 * No server-side components
 * No build process needed
 * Deployable via GitHub Pages
 * Can fetch GitHub Readme files
 * Gorgeous default theme (and it's responsive)
 * Just create an HTML file and deploy!

*Current version: [v0.9.0][dist]*

[![Build Status](https://travis-ci.org/rstacruz/flatdoc.svg?branch=gh-pages)](https://travis-ci.org/rstacruz/flatdoc)

Getting started
---------------

Create a file based on the template, which has a bare DOM, link to the
scripts, and a link to a theme. It will look something like this (not exact).
For GitHub projects, simply place this file in your [GitHub pages] branch and
you're all good to go.

*In short: just download this file and upload it somewhere.*

The main JS and CSS files are also available in [npm] and [bower].

[Default theme template >][template]

[Blank template >][blank]

[bower]: http://bower.io/search/?q=flatdoc
[npm]: https://www.npmjs.org/package/flatdoc

### Via GitHub

To fetch a Github Repository's readme file, use the `Flatdoc.github` fetcher.
This will fetch the Readme file of the repository's default branch.

``` javascript
Flatdoc.run({
  fetcher: Flatdoc.github('USER/REPO')
});
```
