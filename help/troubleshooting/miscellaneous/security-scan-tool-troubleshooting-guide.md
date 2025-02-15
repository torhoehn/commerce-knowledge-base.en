---
title: Adobe Commerce Security Scan tool troubleshooting guide
labels: Adobe Commerce,cloud infrastructure,Security Scan Tool,error,report,admin,troubleshooting,Magento,on-premises
description: "Learn how to troubleshoot the various issues with the  Security Scan tool for Adobe Commerce and Magento Open Source."
---

# Adobe Commerce Security Scan tool troubleshooting guide

Learn how to troubleshoot the various issues with the  Security Scan tool for Adobe Commerce and Magento Open Source.

## Issue: Unable to Submit the site

The Security Scan tool requires that you prove ownership of your site before the domain can be added to the Security Scan Tool. This can be performed by adding a confirmation code to your site using an HTML comment or the `<meta>` tag. The HTML comment should be placed inside the `<body>` tag, e.g., in the footer section. The `<meta>` tag should be placed inside the page's `<head>` section.

A common issue faced by merchants occurs when the Security Scan Tool is unable to confirm the merchant’s site ownership.

If you are getting an error and cannot submit your site for the scan, refer to the [Error message when adding sites into Security Scan](https://support.magento.com/hc/en-us/articles/4531353024013) troubleshooting article in our support knowledge base.

## Issue: Empty reports generated by the Security Scan tool

You get empty scan reports from the Security Scan tool or get reports containing only one error like *Security tool was unable to reach the base URL* or *Magento installation is not found on the provided URL*.

### Solution

1. Check that 52.87.98.44, 34.196.167.176, and 3.218.25.102 IPs are not blocked at 80 and 443 ports.
1. Check the submitted URL for redirects (e.g., `https://mystore.com` redirects to `https://www.mystore.com` or vice versa or redirects to other domain names).
1. Investigate WAF/web server access logs for rejected/unfulfilled requests. HTTP 403 `Forbidden` and HTTP 500 `Internal server error` are the common server responses that cause empty reports generation. Here's an example of the confirmation code that blocks requests by user agents:

```code block
if(req.http.user-agent ~ "(Chrome|Firefox)/[1-7][0-9]" && client.ip !~ useragent_allowlist)

{   error 403;   }
```

You can also see [The Security Scan Tool report is blank](https://support.magento.com/hc/en-us/articles/360029224131-The-Security-Scan-Tool-report-is-blank) article in our support knowledge base for more information.

## Issue: Security issue resolved, but still showing as vulnerable in scan

You resolved a security issue and are expecting the Security Scan to show that you are no longer vulnerable to the newly resolved issue. Instead, you find that the report generated by the Security Scan is still reporting you as vulnerable to the security issue.

### Cause

Cloud instance metadata is gathered only for `active` and `live` Cloud Projects and is NOT a real-time process.

The statistics collection script is run once a day, then the Security Scan tool has to pick up the new data later.

The expected sync-up cycle latency is up to a week and takes a minimum of 24 hours.

The following statuses could appear from checks:

1. **Pass**: The Security Scan tool has scanned your updated data and approved the changes.
1. **Unknown**: The Security Scan tool has no data about your domain yet; wait for the next sync-up cycle.
1. **Fail**: If the status shows fail, you'll need to fix the issue (enable 2FA, change admin URL, etc.) and wait for the next sync-up cycle.

If 24 hours have passed since the changes were made to the instance and they are not reflected in the Security Scan report, you can [submit a support ticket](https://support.magento.com/hc/en-us/articles/360000913794#submit-ticket). Provide the store URL when submitting the ticket.

## BotNet Suspect failure

You receive a notification regarding the "BotNet Suspect" failure.

### Cause

1. The store domain name got into a "Potential BotNet Participants List" back in 2019, and it had the Admin panel, downloader, or RSS functionality publicly exposed, and/or its URL has been mentioned in the CC skimming forums.
1. The alert could be caused by the signs of store compromise and/or malware, like JavaScript found on the page.
1. It is not necessarily an ongoing issue. If the store has been compromised previously, its hostname can still float around the dark web as a 'victim' name.
1. It might also be caused not by Adobe Commerce, but by a system compromise (at the OS level).

### Solution

1. Check for the newly created SSH accounts, filesystem changes, etc.
1. Perform a security review.
1. Check the Adobe Commerce version and upgrade, especially if it’s still running Magento 1, which is not supported anymore.
1. If the issue still persists, [submit a support ticket](https://support.magento.com/hc/en-us/articles/360000913794#submit-ticket) and provide the store URL.

## Issue: Compromise Injection failure

You receive an error regarding a "Compromise Injection" failure.

### Solution

1. Review the scripts indicated in the Security Scan tool report.
1. Review home page source body for inline script injections.
1. Perform system configuration changes review, especially custom `HTML head` and `Miscellaneous HTML` in `footer` section values.
1. Perform code and database review for unfamiliar changes and signs of injected malware.

If none of the above helps, [submit a support ticket](https://support.magento.com/hc/en-us/articles/360000913794#submit-ticket) and provide the store URL and error message from the report.

## Frequently Asked Questions

### Will the Security Scan affect performance on my website?

No. The Security Scan makes all requests one-by-one like a single user. Because of this, the Security Scan shouldn't affect website performance.

### How long does Adobe Commerce keep Security Scan reports?

You can generate the previous 10 reports from your end. If older reports are required, contact Adobe Commerce support. Up to a year of prior Security Scan reports can be obtained.

### What information is needed when submitting a support ticket?

Please make sure to provide the domain name.