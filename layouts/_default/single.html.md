{{ if site.Config.Services.Disqus.Shortname }}
    {{ partial "disqus.html" . }}
{{ end }}
