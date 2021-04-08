---
description: See the effects of changes that you make to endpoints
---

# Detect Breaking Changes

The Live Flows view shows the effects that changes you make in endpoints have on other services using these endpoints.

![](.gitbook/assets/whatsapp-image-2020-11-08-at-15.30.22-dependencies.jpeg)

When you make a change to an endpoint, a _Breaking changes alert_ is shown. This indicates that another flow that uses the endpoint is affected by the change, and may not function correctly as a result.

![](.gitbook/assets/whatsapp-image-2020-11-08-at-15.40.44-breakingchanges.jpeg)

Click on _View change_ to see the response difference.

![](.gitbook/assets/whatsapp-image-2020-11-08-at-15.40.44-breakingchanges-2.jpeg)

Click V_iew dependencies_ to see which other services use this endpoint, and are affected by the change. In the example, two additional services \(_flows-api_, and _aspecto-notifications_\), are affected by the change, with several flows in each.   
Click on the services to investigate further the affected flows

![](.gitbook/assets/whatsapp-image-2020-11-12-at-15.13.27-dependencies-3.jpeg)



{% hint style="warning" %}
If breaking changes never appears, Aspecto might not be running on your service in a [deployed environment](deployed-environment/deployed-environment-set-up.md).
{% endhint %}

