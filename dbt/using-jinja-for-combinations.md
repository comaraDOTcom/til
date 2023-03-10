I want to get the overlap of every table (in terms of percentages of overlapping primary_keys) with other tables. It's equivalent to a cartesian join of a list with itself but dropping all matching combinations in the list. I then want to store the data as a dict, with unique keys and the values of the list of other values in the list per key.

Here's what

As an example let's use cities and I want to measure the overlap of top of a `primary_key` for each city.

To get the list of cities, I'll  run a query in dbt using jinja. I have a table per city.
```sql
{% set get_cities_query -%}
    select distinct
        cities
    from {{ ref('cities') }}
{%- endset -%}

{% set cities_query = run_query(get_cities_query) -%}

{% set distinct_cities = cities_query.columns[0].values() if execute else {} -%}
```
To get the overlap for each city with each other city, construct a dictionary with a key for each city and the values for that key as the list of all the other cities.
```sql
{% set cities_dict = {} %}
{% for city in distinct_cities %}
    {% do cities_dict.update( { city : (distinct_cities | reject("equalto", city | list)}) %}
{% endfor %}
```

Then we'll create a combination of every city's overlap with every other city. We could make this more symmetric, but the macro so calculate the percentage overlap gets the percentage from the primary segment. Assuming a macro called `{{ overlap_stats() }}`. Here we create a CTE per combination.

```sql
with
{% for primary_city, secondary_city_list in city_list.items() %}
    {% for secondary_city in secondary_city_list %}
    {{ primary_city }}_{{ secondary_city }}_overlap_table as (
        {{ overlap_stats(primary_table=primary_city, secondary_table=secondary_key, pimary_key=column_name) }}
    ),
        {% endfor %}
    {% endfor %}
```

Then to get the full overlap we can union them all up.
```sql
cities__overlap as (
	{% for primary_city, secondary_city_list in city_list.items() %}
	    {% for secondary_city in secondary_city_list %}
        select * from {{ primary_city }}_{{ secondary_city }}_overlap_table
        {{ 'union all' if not loop.last}}
        {% endfor %}
        {{ 'union all' if not loop.last}}
    {% endfor %}
)

select * from cities__overlap

```

