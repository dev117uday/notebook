
# URI | URL | URN 

#### The world of Uniform Resource ___

![sum-up](https://media.geeksforgeeks.org/wp-content/uploads/20200205203241/gfg30.jpg)

**Here the word `resource` could be anything, take it as a img on your computer for easier understanding**

### URI 

- `URI` stands for *Uniform Resource Identifier*
- A `URI` contains two things
  - Name
  - Location
- We need URI so identify the resource from its name and also be able to locate it using location

### URL

- `URL` stands for *Uniform Resource Locator*
- This is basically the address of the resource.

### URN

- `URL` stands for *Uniform Resource Name*
- This is basically the name of the resource.


**Therefore : `URI = URL + URN`**

The URI generic syntax consists of a hierarchical sequence of five components

```
URI = scheme:[//authority]path[?query][#fragment]
```

where the authority component divides into three subcomponents:

```
authority = [userinfo@]host[:port]
```

![Wikipedia img](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d6/URI_syntax_diagram.svg/1068px-URI_syntax_diagram.svg.png)

```
          userinfo       host      port
          ┌──┴───┐ ┌──────┴──────┐ ┌┴┐
  https://john.doe@www.example.com:123/forum/q/?tag=networking#top
  └─┬─┘   └───────────┬──────────────┘└───────┬───────┘ └───────────┬─────────────┘ └┬┘
  scheme          authority                  path                 query           fragment
```

I hope you like this article !