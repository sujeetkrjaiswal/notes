---
description: >-
  What is Web Performance, Why is it important and How can you measure it and
  improve it?
---

# Web Performance - What, Why & How

## What is Web Performance?

Web performance is all about making sites fast, including making the slow ones seem fast. It includes both objective measurements like time to load, frames per second, and time to become interactive, and subjective experiences of how long it felt like it took the content to load.

When a user navigates to a page, they are looking out for a certain type of feedback and experience. Below are the most critical questions that the user might have when visiting a site.

* Is it happening?: Did the navigation happen successfully? Has the server responded?
* Is it useful?: Did sufficient content render that I can engage with?
* Is it usable?: Can I interact with the page or is it still loading?
* Is it Delightful?: Are the interactions smooth and natural, free of lag & jitter?

> Ultimately, user-perceived performance is the only performance that matters. - [MDN](https://developer.mozilla.org/en-US/docs/Web/Performance/Fundamentals)&#x20;

When we talk about Web Performance, below are the major factors that are considered.

![Web Performance - Load indicators](../../.gitbook/assets/screenshot-2020-09-13-at-9.12.08-am.png)

#### Startup Performance (Load time)

A lot of people talk in terms of "a site took x.yz sec. to load", but it is not a single moment in time, instead it has multiple moments that can change the perception of a site being "fast" or "slow".

_**Is it happening?**_

* **First Paint** is the point at which sufficient application resources have been loaded to paint an initial frame.
* **First Contentful Paint** is the point at which the first text or image is painted.

_**Is it Useful?**_

* **First Meaningful Paint** is the point at which the primary content of the page has been loaded. (Deprecated in favor of LCP)
* The **Largest Contentful Paint** is the point at which the largest text or image is painted.&#x20;

_**Is it Usable?**_

* **Time to Interactive** is the time it takes to become fully interactive.

#### Runtime Performance

This part of the Performance tries to answer "Is it delightful"

* **Responsiveness** refers to the time taken by a page to respond to user interaction. Generally, it should be between 50ms to 100ms to make sure the user doesn't feel any lag.
* **Frame rate**: Human eye can detect any lag below 60fps and hence when we are adding animations or transition or scrolling to our application, we need to make sure that it happens at >=60fps which will make a better user experience in terms of smoothness.

#### Memory and Power Usage

Improving memory and power usage is as important as improving startup time.&#x20;

Modern CPUs can enter a low-power mode when idle. An application that fires unnecessary timers or keeps unnecessary animations, prevents CPUs from entering low power mode. In a world where most of the users use battery-powered devices such as mobiles, tab, and laptops, power saving is an important factor.

When applications are sent to the background, applications should be able to drop as many loaded resources in memory as possible in order to use less memory when running in the background. You can listen to an event(`visibiltyChange`) and drop the resources from in-memory and if required later,  store in browser storage like indexedDB or just cache in the browser. Application getting pushed to the background is now much more common since the usage over phone and tabs has increased.

## Why is Web Performance Important?

Reducing the download and render time of a site improves user experience and user satisfaction, consequently improving conversion rates and user retention.

Visit "[WPO Stats](https://wpostats.com)" for Case studies and experiments demonstrating the impact of web performance optimization (WPO) on user experience and business metrics.

Here are some real-world examples of performance improvements:

* [Rossignol.com improved their load time by 1.9 seconds and cut their Speed Index by a factor of 10, contributing to a 94% improvement in conversion rate when compared to the year prior.](https://www.fasterize.com/en/blog/fasterize-helps-rossignol-divide-its-web-page-load-time-by-10/)
* [A Google study over millions of page impressions found that when a site meets the recommended thresholds for the Core Web Vitals metrics, users are at least 24% less likely to abandon a page before it finishes loading.](https://blog.chromium.org/2020/05/the-science-behind-web-vitals.html)
*

### How long is too long?

> There are no clear set rules as to what constitutes a slow pace when loading pages, but there are specific guidelines for indicating content will load (1 second), idling (50ms), animating (16.7ms) and responding to user input (50 to 200ms). - [MDN](https://developer.mozilla.org/en-US/docs/Web/Performance/How\_long\_is\_too\_long)

As per google guidelines on "[web vitals](https://web.dev/vitals/)", below are the three major metrics and their time&#x20;

![](../../.gitbook/assets/lcp\_ux.svg)

![](../../.gitbook/assets/fid\_ux.svg)

![](../../.gitbook/assets/cls\_ux.svg)



## How can we measure this?

Real-world performance is highly variable due to differences in users' devices, network connections, and other factors. Lab data and Field data are the two approaches to measure web performance.

**Lab data** is performance data collected within a controlled environment with a predefined device and network settings, while **Field data** (also called **Real User Monitoring or RUM**) is performance data collected from real page loads experienced by your users in the wild.

Lighthouse is the most popular tool to measure Lab data. You can use the below steps measure.&#x20;

1. Open a website you want to measure.
2. Open Chrome Dev Tool and navigate to Lighthouse Tab
3. Select Categories you want to measure (Performance) and Device (Mobile or Desktop)
4. Click the Generate Report button.

Once the report is generated, you will see something similar to the below image which measures the most important pre-defined user-centric metrics.

* First Contentful Paint
* Speed Index
* Largest Contentful Paint
* Time to Interactive
* Total Blocking time
* Cumulative Layout Shift

![](../../.gitbook/assets/screenshot-2020-09-13-at-7.57.54-pm.png)

You can also define custom metrics using the [Performance API](https://developer.mozilla.org/en-US/docs/Web/API/Performance).

To measure Field data, the most common approach is to integrate analytics and tracking library with your application that will take care of reporting all the vital metrics.

> Read [Web Performance Metrics](metrics.md) for exhaustive list of pre-defined metrics, defining custom metrics, measuring and mitigating issues for each.



[Chrome User Experience Report (CrUX)](https://developers.google.com/web/tools/chrome-user-experience-report/) provides metrics showing how real-world Chrome users experience popular destinations on the web.

Read more

> Refer  "[Measuring Web Performance](measuring.md)" for details about measuring web performance, why is it necessary to measure and how can we measure different metrics.

> Refer "[Lighthouse - Auditing your web application](lighthouse.md)" for details about the lighthouse, how to use lighthouse, and how to understand the reports generated by the lighthouse.

## How can we improve web performance?

> Refer [Web Performance Optimizations](wpo.md) for exhaustive list of improvements that can be done.

The performance can be improved by focusing on the below four factors:

#### Reduce overall load time

* This can be achieved using loading only the critical resources on page load and lazy loading all the other resources.
* Using content delivery networks (CDNs) also helps in reducing network latency. CDNs are a network of servers spread across the globe and the request is sent to the geographically nearest server and hence the latency of requests is reduced.
* Caching resources whenever possible

#### Making the site usable as soon as possible

This basically means loading the web assets in an order that enables the user to start using the site as soon as possible. The two most common approaches to address this are:

* Above-the-fold design-pattern, where the priority is given on the first fold of the website which is visible on load, and all the content for that are loaded on priority.
* Server-side rendering is an approach where Single Page applications are rendered in server and directly the rendered HTML is sent to the browser which enables users to view the content even before the SPA applications are bootstrapped on the client.

#### Smoothness and Interactivity

* In order to provide a smooth experience at 60fps or more, Javascript animations should be avoided that uses heavy computations.
* Any interaction on the page should be responded to within 50ms - 200ms in order to make sure there is no lagging.

#### Perceived Performance

When an operation is going to take a longer time, it is essential to keep the user engaged by showing some visual tips and messages.

A lot many times, the user interactions requires making network call which inherently is slow. For such use cases, something similar to a progress bar should be shown to give some visual feedback to a user and hence improving the perceived performance instead of not showing anything at all.&#x20;

## References

* [https://developer.mozilla.org/en-US/docs/Learn/Performance/What\_is\_web\_performance](https://developer.mozilla.org/en-US/docs/Learn/Performance/What\_is\_web\_performance)
* [https://developer.mozilla.org/en-US/docs/Web/Performance/Fundamentals](https://developer.mozilla.org/en-US/docs/Web/Performance/Fundamentals)
* [https://developer.mozilla.org/en-US/docs/Web/Performance/How\_long\_is\_too\_long](https://developer.mozilla.org/en-US/docs/Web/Performance/How\_long\_is\_too\_long)
* [https://web.dev/what-is-speed/](https://web.dev/what-is-speed/)
* [https://web.dev/why-speed-matters/](https://web.dev/why-speed-matters/)
* [https://developer.mozilla.org/en-US/docs/Learn/Performance/why\_web\_performance](https://developer.mozilla.org/en-US/docs/Learn/Performance/why\_web\_performance)
