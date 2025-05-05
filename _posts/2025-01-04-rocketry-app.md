---
layout: post
title: "Rocketry iOS App"
subtitle: "Live Launch Tracker"
category: Web & Apps
src: "assets/2025-01-04-rocketry-app/icon.webp"
---

[rocketry]: https://rocketry.app/
[appstore]: https://apps.apple.com/us/app/rocketry-live-launch-tracker/id6738462173

Today, I'm excited to introduce [**Rocketry**](rocketry), an iOS application built to plan an optimal in-person viewing experience of the latest spaceflight launches.

![Composite Screenshot Group Preview of Rocketry](https://raw.githubusercontent.com/dylanhawley/RocketryWebsite/refs/heads/main/public/Rocketry-Screenshot-Group-All.webp)
<p align="center"><i>Composite screenshot preview of Rocketry, from its App Store page</i></p>

<div align="center">
<a href="https://apps.apple.com/us/app/rocketry-live-launch-tracker/id6738462173">
  <img
    src="https://rocketry.app/Download_on_the_App_Store_Badge_US-UK_RGB_wht_092917.svg"
    alt="Download on App Store"
    width="200"
    height="60"
  />
</a>
</div>

## The Idea

Surprisingly, none of the existing spaceflight news apps prioritize informing users about viewing conditions for rocket launches. As a former Space Coast resident and hobbyist launch photographer, I found myself overwhelmed by the sheer number of launches happening these days. It became clear that having reliable viewing condition forecasts would help me decide when it’s actually worth making the trip—ensuring the best chance for a memorable experience and a stunning photo.

## Building the Product

### Data Sources

[ll2]: https://thespacedevs.com/llapi/
[tsd]: https://thespacedevs.com/supportus/
[wk]: https://developer.apple.com/weatherkit/
[wk-src]: https://developer.apple.com/weatherkit/data-source-attribution/
[sln]: https://spacelaunchnow.me/
[solar]: https://github.com/ceeK/Solar/tree/master/
[ceek]: https://github.com/ceeK

|Data Type|Tool|Creator|
|----|-----|-----|
|Upcoming Launches|[Launch Library 2][ll2]|[The Space Devs][tsd]|
|Weather Forecast|[WeatherKit][wk]|[Apple Weather][wk-src]|
|Sunrise / Sunset|[Solar][solar]|[Chris Howell (ceeK)][ceek]

[Launch Library 2][ll2] is a database that powers big projects such as [Space Launch Now][sln]. The API is accessible to everyone for free. I choose to support them on Patreon to help make their data collection possible, as well as increase my rate limit from **15 calls per hour** to **500 calls per hour**. This can be independently verified on [The Space Devs][tsd] website, where my name is listed under Premium Supporters.

[Solar][solar] is a Swift micro library for generating Sunrise and Sunset times. It performs its calculations locally using an algorithm from the [United States Naval Observatory](http://edwilliams.org/sunrise_sunset_algorithm.htm), and thus does not require the use of a network.

### Weather at a Glance

[hws]: https://www.hackingwithswift.com/plus/remaking-weather

Making the UI beautiful while also information dense was a priority for me. I learned how to create beautiful and fast animations natively in SwiftUI by following [Remaking Weather by Hacking with Swift][hws]. Here's a peek into how just one component of the animated weather backgrounds in [Rocketry][rocketry] work:

1. Fetch the upcoming `Launch` model from the on-device SwiftData cache.
2. Calculate Sunrise / Sunset times using the `time` and `location` of the upcoming launch.
3. Map the Solar event times to gradient stops in a `LinearGradient` representing the colors of the sky based on time of day and location.
4. Interpolate the launch time within the `LinearGradient` and construct a `View` displaying only the nearest gradient stops.

Part of the elegance in this component is rendering most of the weather component without actually needing to fetch weather data. The sky gradient is calculated entirely on-device, making these load instantly. 

Visual components that *require* fetching weather data, such as clouds, can just fade in a few milliseconds after the view is rendered and the user will not notice or care about the delay caused by a network call.

### Widgets

Making a near real time widget on iOS is nearly imposssible. `WidgetKit`, the framework used to construct iOS Widgets, severely limits the number of updates that can be scheduled on the widget's `Timeline`. This means if a launch is delayed, the Rocketry widget will not update the NET until your iPhone feels like its time to fetch new data. This is simply an unfortunate limitation we have to deal with as our hands are tied.

> The Clock widget that comes natively with iOS has an animated second hand that moves in real time. After some research, I learned this is an impossibility to recreate and ship on the App Store. Apple simply gave themselves permission to update native widgets at whatever frequency they determine to be best, while handicapping other developers.

### Code Repository

You can find the project source code [here on GitHub](https://github.com/dylanhawley/Rocketry).

## Feedback

Rocketry aims to make viewing rocket launches a more pleasurable and predictable experience. If you'd like to make a feature suggestion or just reach out, my email is [dylanthomashawley@gmail.com](mailto:dylanthomashawley@gmail.com).

Happy coding (and launch watching)! 