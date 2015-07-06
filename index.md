---
layout: page
title: Abnormal Distributions
---
{% include JB/setup %}

### A little about me:
 I am Saad Mahmood and I just wanna do what I love, and answering challenging questions using data is something I truly enjoy doing.
My knowledge in this area of Data Analysis, Machine Learning, and Statistical Learning largely comes through books.
I have also taken some MOOCs on data analysis and these have provided a foundation upon which more knowledge was built.

I will post the complete list of resources I learnt from soon..

### Structure of the blog:

1. Ask a question
2. Use data to try to answer it
3. Explain the (lack of) solution


#### You can contact me via my e-mail address:

##### sa3d.ma7mood@gmail.com

#### Check my work on GitHub:

##### [github.com/ma7mood](http://github.com/ma7mood)

#### Here's a list of my posts

<ul  class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

