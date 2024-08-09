+++
title = '{{ replaceRE `[-_]` " " .File.ContentBaseName | title }}'
date = {{ .Date }}
lastmod = {{ now.Format "2006-01-02T15:04:05-07:00" }}
draft = false
+++
