---
title: The Cost of Triathlon
tags: [money, fitness]
---

Over the past few years, I've started doing triathons. It started with just
running, but before long I was buying a bike and learning how to swim properly.
It's fun to keep pushing yourself.

In the back of my head, I always knew it was an expensive sport - cycling in
particular - but I never bothered to tally everything up. Until now, that is.

A few notes:

- Prices are what I paid at the time of purchase
- Items are sorted in order of decreasing priority
- Most of this stuff is optional

{% assign expenses = site.data.cost_of_triathlon %}
 
{% for category in expenses %}
#### {{ category.name | capitalize }}
  <table class="table table-sm table-bordered table-striped">
    <thead>
      <tr>
        <th scope="col">No.</th>
        <th scope="col">Item</th>
        <th scope="col">One-time cost</th>
        <th scope="col">Annual cost</th>
        <th scope="col">Notes</th>
      </tr>
    </thead>
    <tbody>

      {% assign one_time_total = 0 %}
      {% assign annual_total = 0 %}
      {% assign i = 0 %}
      {% for item in category.items %}
        {% assign i = i | plus: 1 %}

        <tr>
          <td width="3%">{{ i }}</td>
          <td>
            {% if item.url %}
              <a href="{{ item.url }}">{{ item.name }}</a>
            {% else %}
              {{ item.name }}
            {% endif %}
          </td>
          <td width="18%">{% include usd.html amount=item.one_time_cost %}</td>
          <td width="18%">{% include usd.html amount=item.annual_cost %}</td>
          <td>{{ item.notes | default: "-" }}</td>
        </tr>

        {% assign one_time_total = one_time_total | plus: item.one_time_cost %}
        {% assign annual_total = annual_total | plus: item.annual_cost %}

      {% endfor %}

      <tr>
        <td>-</td>
        <td><b>Total</b></td>
        <td width="18%"><b>{% include usd.html amount=one_time_total %}</b></td>
        <td width="18%"><b>{% include usd.html amount=annual_total %}</b></td>
        <td>-</td>
      </tr>

    </tbody>
  </table>
{% endfor %}


#### Totals
<table class="table table-sm table-bordered table-striped">
  <thead>
    <tr>
      <th scope="col">Category</th>
      <th scope="col">One-time costs</th>
      <th scope="col">Annual costs</th>
    </tr>
  </thead>
  <tbody>

    {% assign one_time_grand_total = "" | split: "," %}
    {% assign annual_grand_total = "" | split: "," %}
    {% for category in expenses %}

      {% assign one_time_total = 0 %}
      {% assign annual_total = 0 %}
      {% for item in category.items %}
        {% assign one_time_total = one_time_total | plus: item.one_time_cost %}
        {% assign annual_total = annual_total | plus: item.annual_cost %}
      {% endfor %}

      <tr>
        <td>{{ category.name | capitalize }}</td>
        <td width="18%">{% include usd.html amount=one_time_total %}</td>
        <td width="18%">{% include usd.html amount=annual_total %}</td>
      </tr>

      {% assign one_time_grand_total
        = one_time_grand_total | plus: one_time_total %}
      {% assign annual_grand_total
        = annual_grand_total | plus: annual_total %}

    {% endfor %}

    <tr>
      <td><b>Total</b></td>
      <td width="18%"><b>{% include usd.html amount=one_time_grand_total %}</b></td>
      <td width="18%"><b>{% include usd.html amount=annual_grand_total %}</b></td>
    </tr>

  </tbody>
</table>
