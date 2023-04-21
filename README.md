Security Risk Dashboard

`*alpha software*`

This is a simple dashboard that shows the security risk of open-source projects. Target audiences include:
- Software Developers -> quickly evaluate the risk of OSS they plan to use in their product
- Security Engineering Teams -> incorporate the rich set of metrics into their risk assessment process
- Engineering Management -> understand ecosystem-wide risk and make informed decisions about OSS usage

The dashboard is customizable and incorporates data from the following sources (more to come):
- OpenSSF Scorecard
- Libraries.io
- git
- GitHub

Usage

`1.` raw data
```
$ dash generate --repo=github.com/project/repo --output=json

{
    "scorecard": { ... },
    "libraries_io": { ... },
    "git": { ... },
    "github": { ... }
}
```

`2.` dashboard
```
$ dash generate --repo=github.com/project/repo --output=html --config=dashboard.yaml
```
sample config file
```
dashboard:
  title: "Security Risk Dashboard"
  description: "This is a sample dashboard"
  metrics:
    - name: "OpenSSF Scorecard"
      attributes: # these are pulled from the raw json output; use jq syntax
        - path: .score
          component: card
        - path: .checks[0].{name: name, score: score}
          component: donut
    - name: "Libraries.io"
      attributes:
        - path: .checks[0].{value: values}
          component: card
```