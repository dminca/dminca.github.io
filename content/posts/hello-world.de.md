---
title: "Hello World"
date: 2021-02-24T22:26:52+01:00
toc: false
images:
comments: true
tags:
  - untagged
---

Probieren wir erstmal die Platforme ob sie in gute zustand ist

Hier ist ein einfacher YAML Dokument :smile:

```yaml
{{- if (and (ne (.Values.ui.ingress.enabled | toString ) "-") .Values.ui.ingress.enabled) }}
{{- $serviceName := printf "%s-%s" (include "vault.fullname" .) "ui" -}}
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: {{ template "vault.fullname" . }}-ingress
  namespace: {{ .Release.Namespace }}
  labels:
    app: {{ template "vault.name" . }}
    chart: {{ template "vault.chart" . }}
    heritage: {{ .Release.Service }}
    release: {{ .Release.Name }}
  {{- if .Values.ui.ingress.annotations }}
  annotations:
{{ toYaml .Values.ui.ingress.annotations | indent 4 }}
  {{- end }}
spec:
  rules:
    {{- range .Values.ui.ingress.hosts }}
    - host: {{ . }}
      http:
        paths:
          - backend:
              serviceName: {{ $serviceName }}
              servicePort: http
    {{- end -}}
  {{- if .Values.ui.ingress.tls }}
  tls:
    {{- range $value := .Values.ui.ingress.tls }}
    - hosts:
      {{- range $value.hosts }}
      - {{ . }}
      {{- end }}
      secretName: {{ $value.secretName }}
    {{- end }}
  {{- end }}
{{- end }}
```

## Ein bisschen Python für die Überraschung Element

```python
#!/usr/bin/python3

from engine import RunForrestRun

"""Test code for syntax highlighting!"""

class Foo:
	def __init__(self, var):
		self.var = var
		self.run()

	def run(self):
		RunForrestRun()  # run along!
```