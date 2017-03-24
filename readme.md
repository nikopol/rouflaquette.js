rouflaquette.js 0.1
===================

yet another vanilla and tiny mustache like template engine.


*sample usages*: 
```js
//basic usage
rfq("hello {{user.name|capital}}", {user:{name:"niko"}}); //-> "hello Niko"
rfq("hello {{name|or:buddy|capital}}", {});               //-> "hello Buddy"
rfq("{{txt|mark:42}}",{txt:"the answer is 42"});          //-> "the answer is <mark>42</mark>"

//templating
var data = {authors:[{name:"asimov"},{name:"dick"},{name:"herbert"}]};
rfq.template({
  'author-line': '<li>{{name}}</li>',
  'authors-list': '<h2>{{authors.length|plural:no author:one author:{} authors}}</h2><ul>{{authors|tpl:author-line}}</ul>'
})('authors-list',data);
// -> "<h2>3 authors</h2><ul><li>asimov</li><li>dick</li><li>herbert</li></ul>"
```

### Commands


#### `rfq(template, vars)`

##### parameters
`template` is a string to parse *or* a registred template name.  
`vars` is an object with keys/values used in the template.

variables in the templates are basically represented `{{varname}}`, but you can also apply filter(s) to them with the syntax `{{varname|filter1|filter2|etc}}`.

##### returns
it returns the resulting string


#### `rfq.template(templates)`

##### parameters
`templates` is an object like `{'template-name':'template {{value}}'}`

##### returns
it returns `rfq`


### Filters

| filters                  | purposes                                                    |
|--------------------------|-------------------------------------------------------------|
| floor                    | convert to number rounded lower                             |
| round                    | convert to number rounded upper                             |
| capital                  | uppercase first letter of each words                        |
| lowercase                | lowercase the whole string                                  |
| uppercase                | send a nuclear duck into space                              |
| subs:from:to             | extract a substring begging at `from` and ending at `to`    |
| replace:what:by          | replace `what` by `by`                                      |
| trim                     | remove beginning and ending spaces                          |
| join:separator           | join values using separator                                 |
| mark:what                | replace `what` by `<mark>what</mark>`                       |
| plural:none:single:multi | apply none, single or multi template depending on the value |
| or:default               | apply `default` if false                                    |
| if:then:else             | apply `then` if true else `else`                            |
| tpl:tplname              | apply a registred template                                  |

you can add your own filter like that :
```js
rfq.filters['concat'] = function(s,arg){ return s+arg; };
rfq("{{txt|concat: isn't it?}}",{txt:"nice"}); -> "nice isn't it?"
```