---
layout: page
title: index
---

<div markdown="1" class="no_wrap">
```
{
    "key" : "svg",
    "val" : {
        "new" : "circle",
        "id" : "foo",
        "cx" : 100,
        "cy" : 100,
        "r" : 20,
        "fill" : "#66ff66"
    }
}
```
{: class="inline_code"}

```
{
    "key" : "tween",
    "val" : {
        "id" : "bar",
        "target" : "#foo",
        "dur" : 1,
        "vars" : {
            "x" : "+= 200",
            "y" : "+= 200",
            "repeat" : -1,
            "yoyo" : "true",
            "paused" : "false"
        }
    }
}
```
{: class="inline_code"}

```
{
    "key" : "clear",
    "val" : "*"
}
```
{: class="inline_code"}
</div>

{% include drawsocket-web.html %}


<style>
    .highlight pre:hover {
        background-color: pink !important;
        cursor: pointer;
    }

    code {
         background-color: inherit;
    }

    .drawsocket-web {
        height: 400px;
        overflow: hidden;
    }
</style>


<script>

    const snippet_code_block = document.querySelectorAll(".highlight");
   
    snippet_code_block.forEach( b => {
        const snippet_code = b.querySelector("code");
        const snippet = JSON.parse(snippet_code.innerHTML);
        b.addEventListener("click", ()=> {
            drawsocket.input(snippet);
        });
    })

</script>