---
layout: page
title: Summer Training 2019
---

# Summer Training 2019

Welcome to Compete McGill's 2019 Summer Training!

Over the course of the Summer, we hope to teach you some of the Core Algorithms used in Competitive Programming. **In particular**, how to **implement them in code**!

Due to limited resources, we can only support **McGill University students or alumni** :cry:, but if that describes you, don't hesitate to take part!

## How does it work?

Every week, we'll assign a section of **[Competitive Programming 3](https://cpbook.net/)** (a.k.a CP3) - the mother of all Competitive Programming books for you to read up **on your own**, as well as [Problem Sets](/problemsets/) for you to
practice on. These problems will be hosted on **[CodeForces](https://codeforces.com/), [Kattis](https://open.kattis.com/) and [UVa](https://uva.onlinejudge.org/)** - some great Competitive Programming sites! 

Obviously, we're also here to provide you with some support on your Competitive Programming Journey. Join us **weekly on Wednesdays from 7PM - 8:30PM Montreal Time** on [our **Discord Server**](https://discord.gg/JhZQWPW) for a Discussion Session
where we'll walk you through any problems you struggled with!

Please also join our **[Facebook Group](https://www.facebook.com/groups/856545804677764/)** and ask any questions that you may have during the week!

## Schedule (So Far...)

<table>
<tr> 
    <th>Week Number</th>
    <th>Start Date</th>
    <th>End Date</th>
    <th>Sections</th>
    <th>Problem Set</th>
</tr>
{% for session in site.data.schedule %}
{% assign settitle = "Problem Set " | append: session.number %}
{% assign matchingsets = site.categories.problemsets | where: "title", settitle %}
{% assign set = matchingsets.first.url %}
{% assign setnumber = matchingsets | size %}
{% assign matchingsolns = site.categories.solutions | where: "title", settitle %}
{% assign solnumber = matchingsolns | size %}
{% assign soln = matchingsolns.first.url %}
<tr>
    <td>{{ session.number }}</td>
    <td>{{ session.start }} </td>
    <td>{{ session.end }} </td>
    <td>
        <ul>
        {% for section in session.sections %} 
            <li> {{ section }} </li>
        {% endfor %}
        </ul>
    </td>
    <td>
        <ul>
            {% if setnumber >0 %}<li> <a href="{{site.baseurl}}{{set}}">Problem Set {{session.number}}</a></li> {% endif %}
            {% if solnumber >0 %}<li> <a href="{{site.baseurl}}{{soln}}">Solution Set {{session.number}}</a></li> {% endif %}
        </ul>
    </td>
</tr>
{% endfor %}
</table>


