# Warehouse tools

Small Python utilities for warehouse order and shipping workflows.

Run the tests with `python3 -m unittest discover -s tests -v`.

## Shipping

`shipping_quote(weight_kg, zone, service="standard")` returns a delivery quote
in dollars, rounded to two decimal places. Zones are `local` and `regional`;
weights must be positive. Services are `standard` (the default) and `express`.
Unknown zones or services and nonpositive weights raise `ValueError`.

Standard shipping costs $5 for local or $8 for regional, plus $1.50 per kg.
Express adds $7 to the standard price in either zone. Existing two-argument
calls continue to return standard quotes.

```python
from warehouse.rates import shipping_quote

shipping_quote(2, "local")                     # 8.0
shipping_quote(2, "regional", service="standard")  # 11.0
shipping_quote(2, "local", service="express")   # 15.0
shipping_quote(2, "regional", service="express")  # 18.0
```

The command-line entry point prints a quote for an order. Omit `--service` for
standard shipping, or select `standard` or `express` explicitly:

```console
$ python3 -m warehouse.cli 2 local
$8.00
$ python3 -m warehouse.cli 2 regional --service standard
$11.00
$ python3 -m warehouse.cli 2 regional --service express
$18.00
```

The CLI rejects unknown services with an argument error.
