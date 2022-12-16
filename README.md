GameStep
=======

## Overview
GameStep is a SDK that converts daily footsteps into in-game coins 

GameStep SDK functions as following:
- Retreiving steps data from Apple HealthKit.
- Converting steps to coins (100 steps = 1 coin).
- Store the coins and steps data permanantly to the phone through Apple Developer Tool ```UserDefault``` class.
- Open up a Coins interface for developer to manipulate the SDK.

### Retreiving steps data from Apple HealthKit.

The original code for accessing the healthkit data is from [azarmshap] on youtube, And it was revised to match the data mapping.

[azarmshap]: https://www.youtube.com/watch?v=ohgrzM9gfvM

The folder ```HealthInfo``` is where the SDK get access the data. The developer need to manually add the Healthkit capability from XCode and sign fo the privacy info list for accessing user's health data.

The file HealthStore contains the major methods like: asking for authorization from the user to get access to healthkit, fetching the steps data in a given date range, and get the today's step count so far whenever the user launches the app. In the current file, the query in the function ```fetchStep``` will fetch the data of today in a 1 hr frequency, which can be further extend to 1 week, 1 month and further, and the frequency can be narrowed to 1 min or 1 sec if necessary. In future versions, we will open up an interface for developer to customize the time and frequency manner when accessing the steps data. It will return the a Healthkit collection which will be further be transformed into a collection of array of Step object, which initialize with ```date```, ```count``` and ```UUID```. The function ```getStep``` will directly return a double that contains today's step so far whenever this function gets called.

The step is used to represent a customized data period of steps. It has attributes like ```step``` ```count```, ```UUID``` and ```date```.

## Installation and Usage
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

### Converting steps to coins (100 steps = 1 coin).

The ```GameStep``` file act as a main controller of the SDK, which it receive the steps data from the healthkit access, handle calls from the developer, and do the converting and storing procedure. 

The ```GameStep``` main class have variables like:
- ```healthstore``` is used to retreive steps from healthkit.
- ```currentStep```, is used to record the newest step retreived from the healthkit.
- ```theCoin``` is used to record the newest coin converted.


## Endpoints
### getCoins()
```func getCoins() -> Int``` is a getter function of the coins that user has earned so far. It fetches from the ```UserDefault``` class.

``` swift
var globalUser = GameStep()
let totalCoins = SKLabelNode(text: "Total Coins Earned: \(globalUser.getCoins())")
```

### getSteps()
```func getSteps() -> Int``` is a getter function of the steps that user has walked from 12am until now. It fetches from the ```UserDefault``` class.

``` swift
var globalUser = GameStep()
let totalSteps = SKLabelNode(text: "You walked : \(globalUser.getSteps()) today")
```

### updateCoins()
```func updateCoins()``` is a function that updates current steps and convert excessive steps into coins. It can be called every time a user logs in, or can be called periodically.

``` swift
var globalUser = GameStep()
let prevSteps = globalUser.defaults.integer(forKey: "steps")
globalUser.updateCoins()
var addedCoins = Int(Int(globalUser.currentStep) - prevSteps)/100
let newCoins = SKLabelNode(text: "Congrats!! You Just earned: \(addedCoins) coins")
```

### consumeCoins()
```func consumeCoin(coin: Int) -> Int``` is a function that consumes the coins earned so far. It will return the number of coins after the deduction.

``` swift
var globalUser = GameStep()
let oldCoins = globalUser.getCoins()
let newCoins = globalUser.consumeCoins(coin: costToRevive)
let newCoinsLabel = SKLabelNode(text: "Now you have \(newCoins) coins")
```
