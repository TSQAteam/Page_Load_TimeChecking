# Measuring Web Page Load Times using JMeter

At last count there were over 65 separate commercial load testing tools out there, but few with the name recognition of the open source program JMeter. Often people will call us up and ask to compare Load Tester with JMeter, but I only had a cursory look at it many years ago, and couldn’t speak from recent first-hand knowledge. So, when someone called me last week asking about JMeter, it seemed like a good opportunity to give it another look.

To compare Load Tester and JMeter my first instinct was to record a simple test case with a browser and then run a ramping load, watching how web page load times changed as the load increases. Using the JMeter recorder I grouped together URL requests into pages, and recorded load times at various load levels. While the Load Tester test took about 15 minutes configure and run, it took several days of research to figure out how to set this up in JMeter. Besides not being familiar with JMeter, there’s no automation, so everything had to be configured manually, for example, setting up remote load generators on EC2 alone took two days.

Once the test was configured, the next challenge was to get consistent results. Web pages that should have been loading in the 2 second range were measured by JMeter as loading in 200-400 seconds, which was easy to double check by accessing the system with real desktop browsers during the test. The numbers were off by a couple of orders of magnitude, but it isn’t fair to criticize JMeter for doing what it doesn’t claim to do. Its common knowledge that JMeter doesn’t actually simulate a browser, but I felt like it was worth mentioning again here since so many people who’ve contacted me assume that it does, or that it doesn’t matter.

JMeter and Load Tester have something in common: they are both written in Java. When I was first writing Load Tester it quickly became apparent that the default Java I/O libraries were barely adequate for load testing. The team at Web Performance has spent many man-years writing a custom I/O library optimized for load testing, including millisecond timing accuracy, the efficiency to simulate thousands of users on a single computer, and the most accurate virtual browser available, that simulates every aspect of a browser except for render time. If accurate web page load times are a priority, there’s simply no way to compare these two programs.

There’s also a separate way to measure web pages load times in JMeter, an option to automatically load all of the resources on a page, but that’s quite different from the load generated from a browser. Never-the-less, I came up with a simple test case consisting of a typical uncached web page, a simple ajax page, and a list of articles on a WordPress blog. There was a one second delay between each page, and a ramping load from 0 to 125 concurrent users. Unfortunately it was very difficult to compare because JMeter measured different values on each test. 

I was hoping to come with specific reasons why the web page load times in the two pieces of software were different, but was stymied by the inconsistent results. Note that the test case parameters were identical, the site under test was identical, the load was generated from the same computer, and there was more than enough bandwidth available.

One advantage of open source software is the community, so I checked to see how other people were solving this problem. The most common recommendation I found for measuring web page load times with JMeter was to simply run the tool to generate load, and then browse the site with a real browser with a speed-measuring browser plugin. That kinda works, but a sampling approach like that will give you inconsistent measurements. The reason? Performance isn’t constant even if load level is constant. Web page load time at a single load level is best represented in a histogram, with some users getting fast load times, some getting slow load times, spread across a range. Unless you record all of the load times, you’ll never get an accurate picture. Add in a ramping load to the mix, and manually measuring a loaded system with desktop browsers will give you random results within the range at each load level. If all you need to do is a ballpark check to see if the site will fall over under load, then this suffices, but if you’re trying to tune a website, inconsistent measurements aren’t helpful.

The focus of this blog post was on web page load times, because in web performance that’s what the user sees, but what JMeter does seem to do well is measure the performance of individual HTTP requests. If you’re a programmer tuning a back-end call, having JMeter call it over and over would be useful, since you can measure performance in terms of requests processed per second. JMeter also supports a wide range of non-HTTP protocols that Load Tester does not, so if you need to generate SMTP traffic, for example, Load Tester isn’t an option.

While I’m sure that JMeter would get faster for me if I used it more often, the fact is that when you use JMeter you’re trading cost for efficiency. Web Performance has spent years speeding up Load Tester so that our customers can get their job done as quickly as possible, in every area from configuring test cases to special reports. When you buy a commercial product such as Load Tester the money you spend is measured against the increase in productivity and capabilities.

FYI , I have uploaded two videos in dropbox related to user manual for capturing page load time for website application by using Jmeter Tool.

- https://www.dropbox.com/s/tla4b3wpbqmmist/Page%20load%20Time%20Testing%20Part%20-%201.g13688.mp4?dl=0  - Part - 1
- https://www.dropbox.com/s/76r9sz7qduwsaxh/Page%20Load%20Time%20Testing%20Part%20-%202.Uh6424.mp4?dl=0 - Part - 2
