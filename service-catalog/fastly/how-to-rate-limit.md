# How to Add a Rate Limit

Fastly's Next-Gen WAF (NGWAF) provides [advanced rate limiting rules]. These
rules count requests from each client that match a set of conditions. When a
client exceeds the configured threshold, NGWAF can block that client's requests
for a chosen duration.

## Create the Rule

To create a rate limiting rule, use the Fastly control panel:
_Security_ -> _Next-Gen WAF_ -> _Rules_.

You can take inspiration from the existing rate limiting rules in the
`crates.io-waf` service.

Remember to change the HTTP status code of the response to 429.

## Understand the Interval

The interval is a counting period, not a delay before NGWAF evaluates the
rule. Rate limits are evaluated throughout the interval as NGWAF's counters
are updated.

For example, a threshold of 100 requests in ten minutes can take effect during
those ten minutes as soon as NGWAF observes the threshold being exceeded. It
does not allow unlimited requests for ten minutes and then evaluate the total.

Enforcement is not guaranteed to happen on the exact request that crosses the
threshold.

[advanced rate limiting rules]: https://www.fastly.com/documentation/guides/next-gen-waf/rules/working-with-advanced-rate-limiting-rules/
