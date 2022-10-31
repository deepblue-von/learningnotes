### html5页面

1.File-Preferences-User snippets 

2.输入html.json

```json
	"Print to html5": {
		"prefix": "h5",
		"body": [
			"<!DOCTYPE html>",
            "<html lang=\"en\">",
            "<head>",
    		"\t<meta charset=\"UTF-8\"\/>",
            "\t<title>$1<\/title>",
      		"<\/head>",
			"<body>\n",
			"\t<script>",
			"\t\t$2",
			"\t<\/script>",
			"<\/body>",
			"<\/html>",
		],
		"description": "Create a html5 page"
```

