---
layout: post
title: "That One Time I Wrote an App for a Watch"
modified:
categories: articles
excerpt: My favorite thing about running & biking is the fact that I can shove as many tacos in my face as I want when I'm done. 
tags: [running, app development, tacos]
image:
  feature: articles/cover-tacos.jpg
date: 2015-02-03T17:06:05-07:00
comments: true
---

My favorite thing about running & biking is the fact that I can shove as many tacos in my face as I want when I'm done. 

Seriously, I love tacos.

When I'm running, all I can think about is tacos. Sometimes, I think about pizza, but mostly it's tacos. I often wonder, "how many tacos can I eat when I get home?".

It has come to the point where I now need  real-time data on how many tacos I've earned during a run. 

So I wrote an app for my Suunto GPS watch, that tells me just that. 

![tacos!](/images/articles/watch-tacos.jpg)

This app could have been so easy - if only I had a heart rate monitor. Then, it's really just calories burned = tacos you can eat. But I don't have one of those. I had to dive deep into the scary world of math. 

Yes. Math. 

I figured I couldn't be the ONLY person in the world that needed real-time tacos earned data. I figured I should probably write this app for all the people of the world, which presented a few challenges. 

### 1. Personal data

Users of the Suunto platform, Movescount, can enter personal data about themselves. Things like age, weight, height, gender, etc. They can also opt out of those shenanigans and just be anonymous. 

First, I need to grab their age and height. This info will come in handy for later calculations. If they don't have any of that information, I assign them some default values of 'average' people. 

I also want to find out if the user has a heart rate monitor, so they can pass go and collect 200 tacos.

{% highlight c %}
/* If the user does not have an age - they are now 18 */
if (SUUNTO_USER_AGE > 10) {
  userAge = SUUNTO_USER_AGE;
}
else {
  userAge = 18;
}

/* If the user does not have a height -  they are now 5'6" */
if (SUUNTO_USER_HEIGHT > 90) {
  userHeight = SUUNTO_USER_HEIGHT;
}
else {
  userHeight = 172;
}

/* If the user has a HR monitor or not */
if (SUUNTO_HR > 30) {
  userKcal = SUUNTO_ENERGY;
}
{% endhighlight %}

### 2. Basal Metabolic Rate

I learned that everyone burns an average amount of calories per day that can be calculated based on their gender, age, height, and weight. In order to properly calculate how many tacos a person can earn on a run, I need to first find their [BMR](http://en.wikipedia.org/wiki/Basal_metabolic_rate) to use as a base number. 

I don't know, whatever. 

Here's a fun math equation for figuring that number out if you have all the numbers.

![BMR equation](/images/articles/bmr-equation.png)

The following code handles that based on information we can get from a user's profile on Movescount.
{% highlight c %}
/* CALCULATE BASAL METABOLIC RATE BASED ON GENDER */

/* Dudes */
if (SUUNTO_USER_GENDER == 1) {
  if (SUUNTO_USER_WEIGHT > 30) {
    userWeight = SUUNTO_USER_WEIGHT;
  }
  else{
    userWeight = defaultMaleWeight;
  }
  userBMR = (13.75 * userWeight) + (5 * userHeight) - (6.75 * userAge) + 66;
}

/* Laaadies */
else {
  if (SUUNTO_USER_WEIGHT > 30) {
    userWeight = SUUNTO_USER_WEIGHT;
  }
  else{
    userWeight = defaultFemaleWeight;
  }
  userBMR = (9.46 * userWeight) + (1.84 * userHeight) - (4.68 * userAge) + 656;
}
{% endhighlight %}

### 3. Metabolic Equivalent

There's this thing called a MET or [metabolic equivalent](http://en.wikipedia.org/wiki/Metabolic_equivalent). This is defined as the ratio of metabolic rate during a specific physical activity to a reference metabolic rate, set by convention to 3.5 ml O<sub>2</sub>·kg<sup>−1</sup>·min<sup>−1</sup>.

What?

All I know is that there is a chart that assignes a MET to an activity, running is an 8 on the MET scale. 

{% highlight c %}
/* Activity Metabolic equivilent*/
activityMET = 8;
{% endhighlight %}

### 4. Duration

All this tacos earned business really comes down to how active you are for how long. Luckily, Suunto gives me that value in seconds. Unfortunately, my big equation needs it in hours.

Fortunately, I can do the math. 

{% highlight c %}
/* Convert seconds to hours*/
activityDuration = SUUNTO_DURATION / 3600;
{% endhighlight %}


### 5. The big stupid equation

To find out how many calories a person burns while running, based on their age, height, weight and gender all you have to do is take their BMR divide it by 24 (to find how many calories they burn in an hour), multiply that number by the MET value and then multiply that number by activity duration. Obviously.

{% highlight c %}

/* Calculate Calories burned based on BMR & MET */
calorieBurn = (userBMR / 24) * activityMET * activityDuration;


/* CALCULATE TACOS */
/* 271 = calories in a Chipotle corn shell taco with black beans, brown rice, lettuce, 
and cheese. */
if (SUUNTO_HR > 30) {
  tacosBurned = userKcal/ 271;
}
else { 
  tacosBurned = calorieBurn / 271;
}

RESULT = tacosBurned;
{% endhighlight %}

Cool, so that's how many calories a person burns on a run, but how many tacos have they earned? That is the burning question.

Actually, the burning question is how many calories are in a taco? Well, that's still up for debate, but let's just say the average healthy runner would go to Chipotle and order a corn soft shell taco with chicken, brown rice, lettuce, and cheese. Because they're healthy like that. They would then consume 271 calories per taco if they went that way. 

### 6. &#161;TACOS EARNED!

Calories burned divided by calories in a taco (271) equals tacos earned! 

This can all be easily solved if the user has a heart rate monitor which just gives you calories burned based on heart rate. Which is probably a more accurate way to calculate things than letting a designer try to do math. 

Here's all the code below for your consumption. You can download the app to your compatible Suunto watch [over at Movescount](http://www.movescount.com/apps/app10071393-Tacos_Earned).

### Happy running!

{% highlight c %}
/* While in sport mode do this once per second */

/* USER DATA */

/* If the user does not have an age - they are now 18 */
if (SUUNTO_USER_AGE > 10) {
  userAge = SUUNTO_USER_AGE;
}
else {
  userAge = 18;
}

/* If the user does not have a height -  they are now 5'6" */
if (SUUNTO_USER_HEIGHT > 90) {
  userHeight = SUUNTO_USER_HEIGHT;
}
else {
  userHeight = 172;
}

/* If the user has a HR monitor or not */
if (SUUNTO_HR > 30) {
  userKcal = SUUNTO_ENERGY;
}

/* CALCULATE BASAL METABOLIC RATE BASED ON GENDER */

/* Dudes */
if (SUUNTO_USER_GENDER == 1) {
  if (SUUNTO_USER_WEIGHT > 30) {
    userWeight = SUUNTO_USER_WEIGHT;
  }
  else{
    userWeight = defaultMaleWeight;
  }
  userBMR = (13.75 * userWeight) + (5 * userHeight) - (6.75 * userAge) + 66;
}

/* Laaadies */
else {
  if (SUUNTO_USER_WEIGHT > 30) {
    userWeight = SUUNTO_USER_WEIGHT;
  }
  else{
    userWeight = defaultFemaleWeight;
  }
  userBMR = (9.46 * userWeight) + (1.84 * userHeight) - (4.68 * userAge) + 656;
}

/* GET ACTIVITY INFORMATION */

/* Activity Metabolic equivilent*/
activityMET = 8;

/* Convert seconds to hours*/
activityDuration = SUUNTO_DURATION / 3600;


/* Calculate Calories burned based on BMR & MET */
calorieBurn = (userBMR / 24) * activityMET * activityDuration;


/* CALCULATE TACOS */
/* 271 = calories in a Chipotle corn shell taco with black beans, brown rice, lettuce, 
and cheese. */
if (SUUNTO_HR > 30) {
  tacosBurned = userKcal/ 271;
}
else { 
  tacosBurned = calorieBurn / 271;
}

RESULT = tacosBurned;
{% endhighlight %}
