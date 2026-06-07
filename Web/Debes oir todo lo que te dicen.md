# CIDSI presents: Debes oir todo lo que te dicen 50pts.
## Description:
Hay reglas que siempre uno debe poner atención asi como de los detalles de los mismos.
#### Webpage:
(Link)[http://200.9.165.32:8091/]

### Steps to solve:
1. Enter the webpage and right click.
2. Select "View Page Source".
3. Inside the webpage we have a redirection in line 8:
```html
    <link rel="stylesheet" href="/static/style.css">
```
4. Click on it.
5. There you have the flag!!!
```html
body {
    background-color: DeepSkyBlue;
    text-align: center;
    display: flex;
    align-items: center;
    flex-direction: column;
}

h1, div, a {
    /* cidsi{comienza_la_competencia}*/ <---flag
    color: white;
    font-size: 3rem;
}
```

**Note:** Don't forget to get the flag's MD5 hash.
