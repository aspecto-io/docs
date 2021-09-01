# How Do You Want to Send Your Data?

To enjoy Aspecto to the fullest, we encourage you to use our SDK to send traces.

{% page-ref page="install-the-sdk/" %}

However, if you already have an existing OpenTelemetry setup and prefer to keep using it, there are a couple of ways to do it.

{% page-ref page="use-w-o-aspecto-sdk.md" %}

{% page-ref page="using-opentelemetry-collector.md" %}

{% hint style="info" %}
The minimum required for our collector to accept your traces is to have the `service.name` resource attribute on your spans and pass the [Aspecto API-Key](https://app.aspecto.io/app/integration/api-key), either through `aspecto.token` resource attribute, or through the `Authorization` HTTP header when making a request to our collector.
{% endhint %}



