+++
fragment = "content"
weight = 100

title = "Serve and monitor Go packages using TXTDirect and Prometheus"
title_align = "left"

[sidebar]
 active = true
+++

At Okkur Labs we use TXTDirect to serve and monitor our Go packages using the `gometa` type. `gometa` redirects the `go get` requests to a Go package that is specified in the `to=` field. This feature lets you have custom import paths for your Go packages. For example, TXTDirect itself is being hosted on GitHub but we use `go.txtdirect.org/txtdirect` as the import path instead of the repository URI.

Also, you can monitor the number of installs and requests to your Go package using TXTDirect's Prometheus metrics. A list of available metrics is documented in [Prometheus Metrics](https://about.txtdirect.org/docs/configuration/#prometheus-metrics).

To learn how to install TXTDirect check the [Getting Started](https://about.txtdirect.org/docs/getting-started/) section of the documentation.
Remember that you also need an instance of CoreDNS or any other DNS Server that supports `TXT` records.

# Configuration

After installing TXTDirect you have to enable `gometa` and `path` types and also Prometheus in the config file. Here's a example config:

```
go.example.test:80 {
 txtdirect {
 enable gometa path # Note.1
 resolver 127.0.0.1:5353 # Note.2
 logfile stdout
 prometheus # Note.3
 }
}
```

Notes:

1. The `gometa` type can work without `path` enabled, but if you want to serve multiple packages under the same subdomain you have to enable the `path` type too.
2. `resolver` is the address of a DNS server like CoreDNS. You have to change it to the address and port you used while running the DNS server instance.
3. If you only use `prometheus` in the config, TXTDirect uses the default values to start an exporter for the metrics. To see the default values or customize them check the [Prometheus Metrics](https://about.txtdirect.org/docs/configuration/#prometheus-metrics).

# Records

After configuring your TXTDirect instance it's time to add the TXT records to your zonefile.
In case you want to serve multiple packages under a subdomain jump to the [Gometa + Path](#gometa--path) section, otherwise, read the following section.
Also, you can check the [Record Examples](https://about.txtdirect.org/docs/examples/) section of the documentation for a list of examples for every type that is available.

> Keep in mind that the example TXT records used in this article also **need** an `A` or `CNAME` record that points to an instance of TXTDirect.

## Gometa

Add a TXT record like the following example to your zonefile to point a subdomain to your Go package using the `gometa` type:

```
_redirect.txtdirect.example.test. IN TXT "v=txtv0;to=https://github.com/txtdirect/txtdirect;type=gometa"
```

This record points `txtdirect.example.test` to TXTDirect's repository hosted on GitHub.

## Gometa + Path

You need a `path` record that manages the incoming requests to a specific subzone and passes the requests that are for your Go packages to their own zones.

The `path` record should look like this example:

```
_redirect.go.example.test. IN TXT "v=txtv0;to=https://about.txtdirect.org;type=path"
```

As you can see the record's `type=` field is set to `path` and `to=` field is set to TXTDirect's homepage in case something goes wrong and fallback gets triggered.

Now you have to add another record that points to your Go package like this example:

```
_redirect.txtdirect.go.example.test. IN TXT "v=txtv0;to=https://github.com/txtdirect/txtdirect;type=gometa"
```

Like the previous example, you can see that the `type=` field is set to `gometa` and it points to TXTDirect's repository using the `to=` field.

> For more detailed information about the `gometa` type and its fields check the [Gometa Specifications](https://about.txtdirect.org/docs/specification/#gometa-type).

---

Now that the records are ready you can use `go get` to test the custom import path. Keep in mind that if you're testing on your local machine and SSL isn't enabled, you have to pass the `-insecure` flag to `go get`.

# Monitoring

TXTDirect supports Prometheus for collecting metrics about each type, host and etc. To learn more about the available metrics and their use case, check the [Prometheus Metrics](https://about.txtdirect.org/docs/configuration/#prometheus-metrics).

In this article, we cover two of the available metrics to monitor the number of install and failures for a `gometa` zone.

Use `txtdirect_redirect_type_count_total` to check the number of redirects aka installs for the `gometa` type on the specefied host. Example query:

```
txtdirect_redirect_type_count_total{host="txtdirect.example.test",type="gometa"}
```

In case of failures, TXTDirect sends the request into a fallback flow which is documented in the [Fallback](https://about.txtdirect.org/docs/configuration/#fallback) section.
To check the number of failures for a gometa record aka your package, use the `txtdirect_fallback_type_count_total` metric. You have to pass the **host**, **record type** and the **fallback type** to this metric. Example query:

```
txtdirect_fallback_type_count_total{host="txtdirect.example.test", type="gometa", fallback="global"}
```

# Conclusion

In this article we covered how to use `gometa` type to serve and monitor Go packages, but if you're looking for more details about other options available for `gometa` or `path` types be sure to checkout [TXTDirect's documentation](https://about.txtdirect.org/docs). We've covered all the available fields for each type in the ["Specification"](https://about.txtdirect.org/docs/specification/) page.
Also if you faced any bugs or issues while using these types or TXTDirect in general or just need a feature added to the project, be sure to open an issue about it on [TXTDirect's GitHub repository](http://git.txtdirect.org/txtdirect).
