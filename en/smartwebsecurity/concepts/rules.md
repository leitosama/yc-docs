# Rules

Using rules, you can define [conditions](conditions.md) for selecting HTTP request, specify [actions](#rule-action) for requests that match these conditions, and prioritize rules.

The rule priority is set as a numeric parameter from `1` to `1000000`.

{% include [preconfigured-rules-priority](../../_includes/smartwebsecurity/preconfigured-rules-priority.md) %}

You can also log information about the traffic matching your conditions, without applying any action to it.

{{ sws-full-name }} provides the following rule types:
* [Basic](#base-rules)
* [Smart Protection](#smart-protection-rules)
* [Web Application Firewall](#waf-rules)

You can learn more in [Managing rules](../operations/#rules).

## Basic rules {#base-rules}

_Basic rule_ is a rule that allows or denies traffic based on specified conditions. It is used for simple traffic filtering based on specific parameters.

Each [security profile](profiles.md) includes a _basic default rule_ with the lowest priority (`1000000`) that allows or denies all traffic.

## Smart Protection rules {#smart-protection-rules}

_Smart Protection_ is a rule that sends traffic, based on specified conditions, for automatic analysis using machine learning and behavioral analysis algorithms. Depending on the selected [action](#rule-action), suspicious requests are sent to [{{ captcha-name }}](../../smartcaptcha/) for additional verification or get blocked.

## Web Application Firewall rules {#waf-rules}

Web Application Firewall rules engage a [WAF profile](waf.md) to analyze traffic for compliance with the [WAF basic rule sets](waf.md#rules-set). Depending on the selected [action](#rule-action), suspicious requests are sent to [{{ captcha-name }}](../../smartcaptcha/) for additional verification or get blocked.

{% include [note-preview-waf](../../_includes/smartwebsecurity/note-preview-waf.md) %}

You can use the following {{ captcha-name }} CAPTCHA options to verify requests compliant with the Smart Protection and Web Application Firewall rules:

{% include [captcha-integration](../../_includes/smartwebsecurity/captcha-integration.md) %}

## Rule actions {#rule-action}

Actions for basic rules:
* _Deny_ traffic whose parameters match the conditions.
* _Allow_ traffic whose parameters match the conditions.

Actions for Smart Protection and Web Application Firewall rules:
* _Full Protection_: Traffic is checked by ML models and behavioral analysis algorithms. Suspicious requests are sent to {{ captcha-name }}.
* _API Protection_: Traffic is checked by ML models and behavioral analysis algorithms. Suspicious requests are blocked.
