---
layout: home
---

## Posts on this site

<div class="contents">

{% for category in site.categories %}
<h3>{{ category[0] }}</h3>
    {% for tag in site.tags %}
<h4>{{ tag[0]}}</h4>
<ul>
            {% for post in tag[1] %}
<li><a href="{{ post.url }}">{{ post.title }}</a></li>
            {% endfor %}
            </ul>
    {% endfor %}
{% endfor %}

</div>

## About me

Self employed Revit specialist architect, living in Budapest, Hungary. I also teach Revit at one of the local universities. 

Contact me about any Revit related question.

## Contact

<ul>
<li><a href = "mailto:gyetpetATmailboxDOTorg"
   onclick = "this.href=this.href
              .replace(/AT/,'&#64;')
              .replace(/DOT/,'&#46;')"
>Email</a></li>
<li><a href="https://github.com/infeeeee">Github</a></li>
<li><a href="https://revitforum.org/member.php/15218-infeeeee">Revit Forum</a></li>
<li><a href="https://forum.dynamobim.com/u/infeeeee">Dynamo Forum</a></li>
</ul>