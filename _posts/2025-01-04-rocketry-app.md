---
layout: post
title: "Rocketry iOS App"
subtitle: "Live Launch Tracker"
category: Web & Apps
src: "assets/2025-01-04-rocketry-app/icon.webp"
---
Today, I'm excited to introduce [**Rocketry**](https://rocketry.app), an iOS application built to bring the latest spaceflight news directly to your fingertips, via SwiftUI and WidgetKit.

## The Problem: Information Overload

Keeping up with upcoming rocket launches can be exciting, but often requires digging through websites or opening dedicated apps just to find the time. Furthermore, if you're planning to watch a launch in person, knowing the weather forecast is crucial, but that information is often separate. We wanted a simpler, more glanceable way to stay informed not just about *when* the next big launch is, but also the *conditions* for viewing it.

## Our Solution: Widgets Everywhere!

Leveraging the power of Apple's `WidgetKit` and `SwiftUI` frameworks, Rocketry delivers key launch information, including weather forecasts, through beautifully designed widgets. Whether you prefer checking your lockscreen or adding a widget to your home screen, Rocketry has you covered. No need to unlock your phone or launch an app – the essential details are right there.

<p align="center">
  <table cellspacing="0" cellpadding="0" style="border-spacing: 0; border: none;">
    <tr style="border: none;">
      <td style="border: none;"><img src="https://github.com/dylanhawley/RocketryWebsite/blob/main/public/English%20%5Ben%5D%20%7C%20iPhone%20-%206.9%22%20Display%20-%201.jpeg?raw=true" width="150" style="display: block;"></td>
      <td style="border: none;"><img src="https://github.com/dylanhawley/RocketryWebsite/blob/main/public/English%20%5Ben%5D%20%7C%20iPhone%20-%206.9%22%20Display%20-%202.jpeg?raw=true" width="150" style="display: block;"></td>
      <td style="border: none;"><img src="https://github.com/dylanhawley/RocketryWebsite/blob/main/public/English%20%5Ben%5D%20%7C%20iPhone%20-%206.9%22%20Display%20-%203.jpeg?raw=true" width="150" style="display: block;"></td>
      <td style="border: none;"><img src="https://github.com/dylanhawley/RocketryWebsite/blob/main/public/English%20%5Ben%5D%20%7C%20iPhone%20-%206.9%22%20Display%20-%204.jpeg?raw=true" width="150" style="display: block;"></td>
      <td style="border: none;"><img src="https://github.com/dylanhawley/RocketryWebsite/blob/main/public/English%20%5Ben%5D%20%7C%20iPhone%20-%206.9%22%20Display%20-%205.jpeg?raw=true" width="150" style="display: block;"></td>
    </tr>
  </table>
</p>


## How it Works

Rocketry fetches up-to-date launch schedules and details from the fantastic [**Launch Library 2 API**](https://ll.thespacedevs.com/docs/). We parse this data and present it cleanly within the widgets. Weather forecasts are sourced directly from Apple Weather, ensuring you have the latest conditions.

## Tech Stack

*   **SwiftUI:** For building the user interface on iOS.
*   **WidgetKit:** The core framework enabling the widget functionality.
*   **Launch Library 2 API:** Our primary data source for spaceflight events.

## Getting Started

Rocketry is designed for iPhone users. You can find the project source code [here on GitHub](https://github.com/dylanhawley/Rocketry).

## Acknowledgements

A big thank you to the providers of the resources that made Rocketry possible:
*   The [**Launch Library 2**](https://ll.thespacedevs.com/docs/) team for their comprehensive API.
*   The Noun Project for the royalty-free [**Rocket Vector Art**](https://thenounproject.com/icon/rocket-680024/).
*   Apple Weather for providing weather forecast data.

## Conclusion

Rocketry aims to make following space exploration easier and more integrated into your daily routine. We hope you enjoy having the latest launch news readily available on your devices!

Happy coding (and launch watching)! 