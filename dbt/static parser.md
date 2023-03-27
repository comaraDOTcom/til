The static parser in dbt is tempermental. It improves compilation time but is limited to work with models using jinja tags: `ref`, `source`, `config`. [dbt parsing docs](https://docs.getdbt.com/reference/parsing) . We've got a large, complex, macro heavy workflow. 

Lately, I've been seeing `I034` and `I037` errors in production logs related static parser failures. There should always be a fallback (I034) error after a static parser failure for successful compilation.
```bash
"code": "I037",
"msg": "static parser failed on <model_path>.sql"
```

```bash
"code": "I034",
"msg": "1602: parser fallback to jinja rendering on <model_path>.sql"
```

Here is the performance of a dbt run command with:
1. Static parser `disabled`.
```
real    5m37.636s
user    2m41.717s
sys     0m14.521s
```

2. Static parser `enabled`
```
real    4m30.754s
user    2m18.800s
sys     0m9.165s
```

Static parser produces a 20% performance increase in the wall clock time. Kernel time is reduced by 35%. Performance is noticeable.